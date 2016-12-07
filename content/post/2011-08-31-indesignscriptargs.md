+++
title = "scriptArgsメモ"
date = "2011-08-31T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignでスクリプトの引数？という scriptArgs があるのでためしてみた

```js
/**
 * InDesign Script Argument memo 
 */

// スクリプト引数をセットする
app.scriptArgs.set("mori", "girl");
app.scriptArgs.set("hayashi", "boy");

if (app.version.split('.')[0] > 5) {
  $.writeln("is valid?: " + app.scriptArgs.isValid);
  // => true
};
// ゲットする
$.writeln("get value of mori : " +   app.scriptArgs.get("mori") );
// => girl

// 保存する
app.scriptArgs.save();

// 消す
app.scriptArgs.clear();
$.writeln("get value of mori : " +   app.scriptArgs.get("mori") );
// => ""

$.writeln("after clear, is defined? : " + app.scriptArgs.isDefined("mori") );
$.writeln("after clear, is defined? : " + app.scriptArgs.isDefined("hayashi") );
// => false

// リストアする（saveしてないとエラーになる）
app.scriptArgs.restore();
$.writeln("and then restore, is defined? : " + app.scriptArgs.isDefined("mori") );
// => true

$.writeln("get value of mori : " +   app.scriptArgs.get("mori") );
// => girl
$.writeln("get value of hayashi : " +   app.scriptArgs.get("hayashi") );
// => boy
```

挙動がわかったけど使いどころがわからない