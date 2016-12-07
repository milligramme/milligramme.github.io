+++
title = "indd配置のオプション"
date = "2015-04-15T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

[Place multiple InDesign document pages \(Importe\.\.\. \| Adobe Community](https://forums.adobe.com/thread/1008925)

indd配置のオプション

app.ImportedPageAttribute -> をつかう

app.pdfPlacePreferences -> PDFはこちら


```js
var doc = app.documents[0];
var t = "/PATH/TO/sample.indd";
app.importedPageAttributes.pageNumber = 8; // => こちらで読み込みするページを決める
var u;

$.writeln(app.importedPageAttributes.pageNumber); // => 8

var o = doc.pages[0].place(
  File(t), u, u, u, u, 
  {importedPageAttributes:{pageNumber:4}} // => 配置時に設定しても効かない
);
$.writeln(o.constructor.name); // => 配置したinddは配列で返る
$.writeln(o[0].importedPageAttributes.pageNumber); // => 8
```