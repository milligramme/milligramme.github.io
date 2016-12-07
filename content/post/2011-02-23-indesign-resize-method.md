+++
title = "違うページサイズを制御するスクリプトメモ"
date = "2011-02-23T00:00:00+09:00"
tags = ["indesign"]
+++
InDesign CS5 で実装された異なるページサイズをスクリプトで生成してみる試み
まず、ページサイズを変更するに当たって、Pageオブジェクトのプロパティにサイズを変更出来そうなものが見当たらない（ boundsは読み込み専用 ）。となると、あとはメソッドの transform() があやしい。
transformationMatricesを作って、試したらあっさりいけた。
サイズはダイレクトに指定するのでなく、今のオブジェクトに対して拡大縮小という形になります。既に変形している場合は前述の boundsで大きさを割り出しておく。
ページをずらすオフセット値？については、mm単位環境にしててもポイント換算になるので、予め変換を考慮しておく必要があります。
サンプルのスクリプトを実行すると

![/images/2011/02/diff_page_test.png](/images/2011/02/diff_page_test.png)

と見開きページの間に違うサイズのページを差し込みます。

ついでにベージにシアーと回転までできるという穴を発見。

![/images/2011/02/diff_page_nandeyanen.png](/images/2011/02/diff_page_nandeyanen.png)

こちらは役に立ちません、PDFに書き出しても残念な結果に終ります。
![/images/2011/02/diff_pdf_nandeyanen.png](/images/2011/02/diff_pdf_nandeyanen.png)

**参照**
 [No.01 異なるページサイズが設定可能に | InDesign CS5 | InDesignの勉強部屋](http://study-room.info/id/studyroom/cs5/study01.html)  

```js

/**
 * custom multiple page size document 
 */

var doc = app.documents.add();
var ddp = doc.documentPreferences;
with(ddp){
  pagesPerDocument = 7;
  facingPages = true;
  allowPageShuffle = false;
}
var doc_w = ddp.pageWidth;
var doc_h = ddp.pageHeight;

// set value
var set_w = 30;  // page width
var set_h = 210; // page height
var offset_h = 50;  // offset horizontal (unit is PT) not works?
var offset_v = -72; // offset vertical (unit is PT)
var shear_ang = 0;  // shear (not recommend)
var rotate_ang = 0; // rotate (not recommend)

var spr_obj = doc.spreads;
spr_obj.everyItem().allowPageShuffle = false;

for (var spi=0, spiL=spr_obj.length; spi < spiL ; spi++) {
  // if facing page
  if (spr_obj[spi].pages.length > 1) {
    // insert a page to middle
    spr_obj[spi].pages.add(LocationOptions.AFTER, spr_obj[spi].pages[0]);
    var target_page = spr_obj[spi].pages[1];

    // create transformation matrix
    var mtrx = app.transformationMatrices.add({
      horizontalScaleFactor : (set_w / doc_w),
      verticalScaleFactor : (set_h / doc_h),
      horizontalTranslation : offset_h,
      verticalTranslation : offset_v,
      clockwiseShearAngle : shear_ang,
      counterclockwiseRotationAngle : rotate_ang
      });

    // change page size 
    target_page.transform(
      CoordinateSpaces.SPREAD_COORDINATES,
      AnchorPoint.TOP_LEFT_ANCHOR,
      mtrx
      );
  };
};
```