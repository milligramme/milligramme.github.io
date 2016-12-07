+++
title = "違うページサイズをresize（）メソッドで制御する"
date = "2011-02-24T00:00:00+09:00"
tags = ["indesign"]
+++

昨日の 「 [[InDesign CS5]違うページサイズを制御するスクリプトメモ](http://www.milligramme.cc/wp/archives/3778)  
」で、transform( )メソッドを使ったページサイズ変更について  @InTools の Harbs. さんに** resize( )メソッド**を使えばいいと教えてもらったので調べてみました。

page.resize( ) では、リサイズ方法を複数のメソッドから選択できて確かにコントロールしやすい。

- ResizeMethods.ADDING_CURRENT_DIMENSIONS_TO, //加算
- ResizeMethods.MULTIPLYING_CURRENT_DIMENSIONS_BY, //乗算
- ResizeMethods.REPLACING_CURRENT_DIMENSIONS_WITH, //指定幅
- ResizeMethods.RESHAPING_AREA_TO_RATIO, //面積を維持して比率
- ResizeMethods.RESHAPING_BORDER_TO_RATIO //境界？？を維持して比率


最後のRESHAPING_BORDER_TO_RATIOの挙動がよくわからない（RESHAPING_AREA_TO_RATIOに似ているけど微妙に違う）

ESTKのヘルプには


> ResizeMethods.RESHAPING_BORDER_TO_RATIO  (Read Only) 
>
> Data Type: number , Value: 1215264595 
>
> Adobe InDesign CS5 (7.0) Object Model 
>
> Change width to height ratio keeping the current perimeter

とあるがcurrent perimeter = 境界? って何だろう？

また

定規単位を考慮するオプションがあるのですが、あまりうまくいかないので。

やはり単位はPT換算にしないといけないぽい。

まとめて実行してみると

![/images/2011/02/diff_page_resize.png](/images/2011/02/diff_page_resize.png)

こんな感じ。

```js

/**
 * custom multiple page size document 
 * using resize () method
 */

var doc = app.documents.add();
var ddp = doc.documentPreferences;
with(ddp){
  pagesPerDocument = 12;
  facingPages = true;
  allowPageShuffle = false;
}

var spr_obj = doc.spreads;
spr_obj.everyItem().allowPageShuffle = false;

// remove first page
doc.pages[0].remove();
for (var spi=0, spiL=spr_obj.length; spi < spiL ; spi++) {
  // if facing page
  if (spr_obj[spi].pages.length > 1) {
    // insert a page to middle
    spr_obj[spi].pages.add(LocationOptions.AFTER, spr_obj[spi].pages[0]);
    var target_page = spr_obj[spi].pages[1];

    // Page.resize (
    //   in:varies, (CoordinateSpaces や BoundingBoxLimitsを指定)
    //   from:varies, (アンカー位置)
    //   by: ResizeMethods , （加算、乗算、指定サイズ、比率など）
    //   values:Array of varies, （メソッドに合わせた値）
    //   resizeIndividually: Boolean , （個別に変形？ オプション、デフォルトはtrue） 
    //   consideringRulerUnits: Boolean （定規の単位を考慮、無視されてるっぽい。オプション、デフォルトはfalse） 

    // 現在のサイズに加算（単位はPT）
    if (spi%5 == 0) {
      target_page.resize(
        CoordinateSpaces.SPREAD_COORDINATES,
        AnchorPoint.TOP_LEFT_ANCHOR, 
        ResizeMethods.ADDING_CURRENT_DIMENSIONS_TO, //加算
        [-200,90], // unit is PT
        true,
        true
        );

    }
    // 現在のサイズに乗算
    else if (spi%5 == 1) {
      target_page.resize(
        CoordinateSpaces.SPREAD_COORDINATES,
        AnchorPoint.TOP_LEFT_ANCHOR,
        ResizeMethods.MULTIPLYING_CURRENT_DIMENSIONS_BY, //乗算
        [2,.3],  //wを2倍、hを0.3倍
        true,
        false
        );
    }
    // 指定サイズに変形（単位はPT）
    else if (spi%5 == 2) {
      target_page.resize(
        CoordinateSpaces.SPREAD_COORDINATES,
        AnchorPoint.TOP_LEFT_ANCHOR,
        ResizeMethods.REPLACING_CURRENT_DIMENSIONS_WITH, //指定幅
        [200,1000],  // unit is PT
        true,
        true
        );
    }
    // 面積を維持して比率を指定
    else if (spi%5 == 3) {
      target_page.resize(
        CoordinateSpaces.SPREAD_COORDINATES,
        AnchorPoint.TOP_LEFT_ANCHOR,
        ResizeMethods.RESHAPING_AREA_TO_RATIO, //面積を維持して比率
        [9,16], 
        true,
        false
        );
    }
    // 境界を維持して比率を指定？？？
    else if (spi%5 == 4) {
      target_page.resize(
        CoordinateSpaces.SPREAD_COORDINATES,
        AnchorPoint.TOP_LEFT_ANCHOR,
        ResizeMethods.RESHAPING_BORDER_TO_RATIO, //境界？？を維持して比率
        [9,16], 
        true,
        false
        );
    }
  };
};
```
