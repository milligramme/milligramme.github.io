+++
title = "Adobe Swatch Exchange file（.ase）の使い道"
date = "2009-08-26T00:00:00+09:00"
tags = ["indesign"]
+++
adobeの [kuler](http://kuler.adobe.com/) のサイトでは、様々なカラーテーマをAdobe Swatch Exchange file（.aseファイル）としてダウンロードすることができます。

ダウンロードした.aseファイルはカラースペースがRGBですので、そのままでは、DTP向きではありません。

CS4だとkulerパネルがありますが、その辺りの処理はどうなんでしょう？

kulerやIllustrator、Photoshopからのase

![/images/2010/09/591-ase.png](/images/2010/09/591-ase.png)

InDesign CS3からのase

![/images/2010/09/594-ase_Indesign.png](/images/2010/09/594-ase_Indesign.png)

InDesign CS3でスウォッチを.aseや.inddから読み込むついでに、カラースペースがRGBならCMYKにして、変換で端数が出るので整数にまるめて、名前も「#RRGGBB」になっているのを「C=00 M=00 Y=00 K=00」（カラー値を持つ名前）にしてみるスクリプトをかいてみました。

### おまけ

.inddドキュメント、.aseファイルからカラー・スウォッチを読み込み、RGBならCMYK変換、スウォッチ名もCMYKにして、選択中のオブジェクトがあればランダムに塗るスクリプト。

![/images/2010/09/592-import_kuler.png](/images/2010/09/592-import_kuler.png)

うーん、ランダム具合がいまいち。

明日はDTP Booster 005に参加します。おやつ必須だそうです。

```js
/*
.inddドキュメント、.aseファイルから
カラー・スウォッチを読み込み、RGBならCMYK変換、スウォッチ名もCMYKに
（して、選択中のオブジェクトがあればランダムに塗り）
"import swatches and convert cmyk"
動作確認：OS10.4.11 InDesign CS3
milligramme(mg)
www.milligramme.cc
*/
if(app.documents.length!=0){
  var docObj=app.documents[0];
  var importSwatchFile=File.openDialog (".inddドキュメントまたは.aseファイル（Adobe Swatch Exchage file）を選択","");
  if(importSwatchFile!=null){
    docObj.loadSwatches (importSwatchFile);
  }
  //RGBスウォッチをCMYKに
  var swObj=docObj.swatches;
  var colorObj=docObj.colors;
  for(var i=0; i < swObj.length; i++){ //スウォッチの内容が[なし]とグラデーション以外を処理 //getElements()の配列から //[なし]はSwatch、グラデーションはGradientをかえしてくる if(swObj[i].getElements()[0].constructor.name == "Color"){ var colorOfSwatch=colorObj.itemByName(swObj[i].name); if(colorOfSwatch.space == ColorSpace.RGB){ //CMYK変換 colorOfSwatch.space=ColorSpace.CMYK; var CMYK=colorOfSwatch.colorValue; //RGB→CMYK変換の変換誤差をまるめる var textC=parseInt(CMYK[0]).toString(); var textM=parseInt(CMYK[1]).toString(); var textY=parseInt(CMYK[2]).toString(); var textK=parseInt(CMYK[3]).toString(); //名前を「カラー値を持つ名前」にする、既にある場合にはママ try{ colorOfSwatch.name="C="+textC+" M="+textM+" Y="+textY+" K="+textK; }catch(e){} //CMYK値を調整 var intCMYK=[eval(textC),eval(textM),eval(textY),eval(textK)]; swObj[i].colorValue=intCMYK; } }//if not None&amp;Gradient }//for swatch convert //ここからは蛇足 //選択しているオブジェクトがあれば、それに現在のスウォッチからランダムに塗ります var selObj=app.selection; if(selObj.length > 0){
    var fillColorOk=confirm ("選択中のオブジェクトを現在のスウォッチからランダムに塗ってもいいですか")
    if(fillColorOk == true){
      for(var j=0; j < selObj.length; j++){ //スウォッチの上位4つが[なし]、[紙色]、[黒]、[レジストレーション]  になっている前提で、それらを除外 if(swObj.length > 4){
        var swatchIndex=4+Math.round(Math.random()*(swObj.length-5));
        selObj[j].fillColor=swObj[swatchIndex];
      }
    }
  alert("fin");
  }
}//蛇足ここまで
}//if document exist
```