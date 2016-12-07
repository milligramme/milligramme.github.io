+++
title = "tables.everyItem（）とcells.everyItem（）"
date = "2014-10-21T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

`everyItem()` でのコンテンツのとれ方メモ

```js
#include "underscore.js"
var doc = app.documents.add();

var tf = doc.textFrames.add({geometricBounds:[0,0,'210mm','297mm']})
var tbl1 = tf.parentStory.tables.add();
var tbl2 = tf.parentStory.tables.add();

_.each(tbl1.cells, function (c) {
  c.contents = "1-" + c.name;
});
_.each(tbl2.cells, function (c) {
  c.contents = "2-" + c.name;
});

;
tf_ev = doc.textFrames.everyItem();
$.writeln(tf_ev.contents.toSource());

table_ev = doc.textFrames.everyItem().tables.everyItem();
$.writeln(table_ev.contents.toSource());

cell_ev = doc.textFrames.everyItem().tables.everyItem().cells.everyItem();
$.writeln(cell_ev.contents.toSource());

// ["\x16\x16"]
// [
//   ["1-0:0", "1-1:0", "1-2:0", "1-3:0", "1-0:1", "1-1:1", "1-2:1", "1-3:1", "1-0:2", "1-1:2", "1-2:2", "1-3:2", "1-0:3", "1-1:3", "1-2:3", "1-3:3"],   ["2-0:0", "2-1:0", "2-2:0", "2-3:0", "2-0:1", "2-1:1", "2-2:1", "2-3:1", "2-0:2", "2-1:2", "2-2:2", "2-3:2", "2-0:3", "2-1:3", "2-2:3", "2-3:3"]
// ]
//
// ["1-0:0", "1-1:0", "1-2:0", "1-3:0", "1-0:1", "1-1:1", "1-2:1", "1-3:1", "1-0:2", "1-1:2", "1-2:2", "1-3:2", "1-0:3", "1-1:3", "1-2:3", "1-3:3", "2-0:0", "2-1:0", "2-2:0", "2-3:0", "2-0:1", "2-1:1", "2-2:1", "2-3:1", "2-0:2", "2-1:2", "2-2:2", "2-3:2", "2-0:3", "2-1:3", "2-2:3", "2-3:3"]

```