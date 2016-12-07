+++
title = "文字列をカラーに変換"
date = "2009-04-05T00:00:00+09:00"
tags = ["extendscript", "indesign"]

outdated = true
+++

InDesignCS2からの機能の「データ結合」は、お手軽自動組版みたいなものですが、 データソースとしてテキストとリンク画像の2種類しか扱えなく、色（スウォッチカラー）を読み込みすることができない。

なので、テキストフレームに文字列として CMYK 値を入れておいて、その色に染めてみる。

データの整形は FileMaker Pro を使います。カテゴリーごとに色が決まっている、商品ラベル制作の時とか用。

![/images/2010/09/344-string_to_color1.png](/images/2010/09/344-string_to_color1.png)

![/images/2010/09/345-string_to_color2.png](/images/2010/09/345-string_to_color2.png)

![/images/2010/09/346-string_to_color3.png](/images/2010/09/346-string_to_color3.png)

```js
/*
文字列で色を塗る
"fill colors by text frames contents"
使い方：
データ結合でテキストフレーム内に書かれた文字列（CMYK値）をもとに色をつけます。
データ結合の後処理用に書いたので、想定条件を限定してます。
* CMYK値はカンマでセパレートしていること
* テキストの数値が０から１００の範囲内であること（FileMaker・Excelなどで整形済み）
* スクリプトラベルのついたオブジェクトに限定
* テキストフレームの塗りのみに対応、線は未対応
* グループ化してないこと
* オーバーフローしていない事（データ結合のときにフォントサイズ1ptしておくで対応）
* 値のテキスト、スクリプトラベルは削除されます
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
(function (){
  var docObj = app.documents[0];
  var pageObj = docObj.pages;
  var scrpLabel = "konpeto";
  for (var i = 0; i < pageObj.length; i++){
    var pageItm = pageObj[i].pageItems;
    for (var j=0; j < pageItm.length; j++){
      if(pageItm[j].label == scrpLabel){
        var colorTxt = pageItm[j].contents.split(",");
        var c = parseFloat(colorTxt[0]);
        var m = parseFloat(colorTxt[1]);
        var y = parseFloat(colorTxt[2]);
        var k = parseFloat(colorTxt[3]);
        var setColor = [c,m,y,k];
        var colorName =
          "C=" + c.toString()+
          " M=" + m.toString()+
          " Y=" + y.toString()+
          " K=" + k.toString();
      if(docObj.swatches.item(colorName) == null){
        var colObj = docObj.colors.add ({
          name: colorName,
          model: ColorModel.process,
          space: ColorSpace.cmyk,
          colorValue: setColor
          });
      }
      pageItm[j].fillColor = docObj.swatches.item(colorName);
      pageItm[j].contents = "";
      pageItm[j].contentType = ContentType.graphicType;
      pageItm[j].label = "";
      } // if scriptlabel
    } // for j
  } // for i
})();
```