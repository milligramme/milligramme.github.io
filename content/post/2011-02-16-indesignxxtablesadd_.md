+++
title = "表の追加（xx.tables.add（） ）の挙動の違い"
date = "2011-02-16T00:01:00+09:00"
tags = ["indesign"]
+++
InDesignで表組みをテキストフレーム内に何個か生成するとき、親オブジェクトをストーリーにするかテキストフレームにするかで、作成される挙動が違っていたのでメモ。
ついでに、オブジェクト属性を Rectangle から TextFrame に ContentType切り替えしても反映されないのメモも。

```js

/**
 * insert tables 
 */

var doc = app.documents.add();
var texf2texf = doc.textFrames.add({
  geometricBounds : [20,20,120,80]
});
// ストーリー内に表を作る
var story_obj = texf2texf.parentStory;
var tb1_a = story_obj.tables.add();
var tb2_a = story_obj.tables.add();
var tb3_a = story_obj.tables.add();

// 一つ目の表セルを3列に、色をシアンにする
tb1_a.columnCount = 3;
tb1_a.cells.everyItem().fillColor = "Cyan";

// 四角形をテキストフレームに変換する
// var rect2texf = doc.textFrames.add({
var rect2texf = doc.rectangles.add({
  geometricBounds : [20,105,120,165]
})
// 四角形はデフォルトでオブジェクト属性は「割当なし」
$.writeln(rect2texf.contentType === ContentType.UNASSIGNED); // => true

// オブジェクトの属性を「割当なし」から「テキスト」に変更する
rect2texf.contentType = ContentType.TEXT_TYPE;

// 変更が反映されない
$.writeln(rect2texf.constructor.name); // => Rectangle

// ビンタ
rect2texf = rect2texf.getElements()[0];

// テキストに属性変更された
$.writeln(rect2texf.constructor.name); // => TextFrame

// テキストフレームにダイレクト？に表を作る
var textframe_obj = rect2texf;
var tb1_b = textframe_obj.tables.add();
var tb2_b = textframe_obj.tables.add();
var tb3_b = textframe_obj.tables.add();

// 一つ目の表セルを3列に、色をマゼンタにする
tb1_b.columnCount = 3;
tb1_b.cells.everyItem().fillColor = "Magenta";
```

実行すると

![/images/2011/02/add_tables.png](/images/2011/02/add_tables.png)

色をつけた一つ目の表の位置が違います。
テキストフレームの**ストーリーに**対して生成された**表は後ろ**に、
**テキストフレームに**ダイレクトに生成すると表は**先頭に**追加されます。

また四角形をテキストフレームに変換というオブジェクトの属性変更は、
GUIでは「四角形に文字ツールでクリック」「オブジェクトメニュー > オブジェクトの属性 > テキスト」などと簡単にできますが、スクリプトの実行中では属性変更がうまく反映されないので getElements()メソッドで自身から配列を取得してその一つ目（[0]）を取り出すということが必要。（四角形なら最初からテキストフレームつくればいいけど、楕円形や多角形のテキストフレーム作りたいときに役に立つかも）