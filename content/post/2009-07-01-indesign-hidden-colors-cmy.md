+++
title = "隠れたC、M、Yカラー"
date = "2009-07-01T00:00:00+09:00"
tags = ["indesign"]
+++
InDesignCS2でSwatch（スウォッチ）とColor（カラー）の違いを知りたくて、新規ドキュメントを作成して、未使用カラーを削除すると、Swatch は4つ。
でも、JavaScript で調べて見ると、Color は9つ。その差5つは何？

```js
var docObj = app.activeDocument;
var colorObj = docObj.colors;
var swatchObj = docObj.swatches;
alert("color length:" + colorObj.length + "\r" + "swatch length:" + swatchObj.length);
for(var i=0; i < colorObj.length; i++){
    $.write("\r" + i + 
    "\rname:"+colorObj[i].name + 
    "\rcolor value:" + colorObj[i].colorValue +
    "\rid:" + colorObj[i].id +
    "\rmodel:" + colorObj[i].model + 
    "\rspace:" + colorObj[i].space
    );
}
```

![/images/2010/09/505-swatch_and_color.png](/images/2010/09/505-swatch_and_color.png)

9つの Color の内容してみると...4つの Swatch の他に

```
1
name:Cyan
color value:100,0,0,0
id:12
model:1886548851
space:1129142603

2
name:Magenta
color value:0,100,0,0
id:13
model:1886548851
space:1129142603

5
name:Yellow
color value:0,0,100,0
id:17
model:1886548851
space:1129142603

6
name:
color value:0,0,0,72
id:113
model:1886548851
space:1129142603

8
name:
color value:0,0,0,100
id:117
model:1886548851
space:1129142603
```

使ってなくても、Cyan, Magenta, Yellow が予約されているみたい。あと2つ、謎の名無し Color が K=72 と、Black じゃない K=100。ドキュメントデフォルト？　どこかで使っている？　前回使っていた履歴が残っている？CS3 でも試したらやはり、数が違うけど名無し Color が存在するみたい。結局わからず。とりあえず、C,M,Y は必ず存在するということは、ドキュメントに有る無しを気にせず、"Black", "Paper" のような感覚で使えるという事なのですね。知らなかった...


090710Wed1700ころ追記

隠しカラーを使うには

```
obj.fillColor=app.activeDocument.colors.itemByName("Cyan");
```

とやると使えます。

```
obj.fillColor=app.activeDocument.swatches.itemByName("Cyan");
```

とやると「そんなスウォッチはない」と怒られます。

090710Wed2200ころ追記
  
お〜まちさんのコメントにあるよう、前回使用したカラーなども名無しカラーとして保存されるようです。