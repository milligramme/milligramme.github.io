+++
title = "ヘルパーオブジェクト.colorPicker（）の使い道"
date = "2010-02-07T00:00:00+09:00"
tags = ["extendscript"]
+++
ExtendScriptの $オブジェクト（ヘルパーオブジェクト）は地味に便利です。
スクリプトを書くとき、デバッグに役立つのが $.writeln(message)　や $.bp()　や $.sleep() といったメソッド、また、多国語仕様にするときに役立つ $.localize などがあります。
そんな中に、実行途中でプレビューしながら色をつくるのに使えそうメソッドが
$.colorPicker();
ということで試してみました。たぶん、InDesignでもIllustratorでもPhotoshopでも使えます。

OSXの場合、こんなカラーピッカーが表示されます。

![/images/2010/09/725-osx_color_picker.png](/images/2010/09/725-osx_color_picker.png)

```js
//スクリプトの途中で色をプレビューしながら生成する
#target 'indesign';
var doc = app.documents.add();
var obj = doc.rectangles.add({geometricBounds:[20,20,150,150]});
var pick = $.colorPicker ();
if(pick == -1){exit();}
var pick16 = pick.toString(16);
//Rが一桁のとき色が大ずれするのを回避
if(pick16.length == 5){pick16="0"+pick16;}
col = doc.colors.add ({
  space:ColorSpace.RGB,
  colorValue:[
    parseInt(pick16.substr (0, 2), 16),
    parseInt(pick16.substr (2, 2), 16),
    parseInt(pick16.substr (4, 2), 16)
    ]});
//ここからRGBからCMYK変換、色が転びます
/*
  col.space = ColorSpace.CMYK;
  var cmyk = col.colorValue;
  var valC = Math.round(cmyk[0]);
  var valM = Math.round(cmyk[1]);
  var valY = Math.round(cmyk[2]);
  var valK = Math.round(cmyk[3]);
  col.colorValue = [valC, valM, valY, valK];
  var nameC = "C="+Math.round(valC)
  var nameM = " M="+Math.round(valM)
  var nameY = " Y="+Math.round(valY)
  var nameK = " K="+Math.round(valK)
  col.name = nameC+nameM+nameY+nameK;
*/
//CMYK変換ここまで
obj.fillColor = col;

```

.colorPicker() の戻り値がRGBなのでカラーピッカーでCMYKを選んでも、CMYK〜RGB〜CMYKと変換されるので選んだ通りの数値になりません。Kulerもいっしょ。