+++
title = "親（parent）さがしメモ"
date = "2009-04-30T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

 [http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2930](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2930) 

梅花藻  さんのコメントがわかりやすかったので、メモ。

constructor.nameとかで手探りに何々？とちくちくエラーだしながらの学習法に不安を感じていたのでちょっと安心。「○○を●●に変更」というのが新鮮。

```js
var sel = app.activeDocument.selection;
var tmp0 = sel[0].parent;
alert("sel[0].parent = "+tmp0.constructor.name);

//tmp0.textFrames[0]をsel[0].parentTextFrames[0]に変更
var tfobje = sel[0].parentTextFrames[0];
app.activeDocument.selection = tfobje;
alert("sel[0].parentTextFrames[0] = "+tfobje.constructor.name);

var tmp1 = tfobje.parent;
app.activeDocument.selection = tmp1;
alert("tfobje.parent = "+tmp1.constructor.name);

//tmp1.groups[0]をtmp1.parentに変更
var gpobje = tmp1.parent;
app.activeDocument.selection = gpobje;
alert("tmp1.parent = "+gpobje.constructor.name);
```