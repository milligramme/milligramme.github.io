+++
title = "doc.save（） の返り値"
date = "2012-06-04T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

初歩的なところでこけたのでメモ

### エラーになる

```js
#target 'indesign'

var doc = app.documents.add();
var doc_path = "~/Desktop/a.indd";
doc.save(File(doc_path));
var tf;
try {
  tf = doc.textFrames.add();
}
catch(e){
  $.writeln(e); // => ReferenceError: オブジェクトが無効です
}
```

### エラーにならない

```js
#target 'indesign'

var doc = app.documents.add();
var doc_path = "~/Desktop/a.indd";
doc = doc.save(File(doc_path));
var tf;
try {
  tf = doc.textFrames.add();
  $.writeln(tf.id);  // => 123
}
catch(e){
  $.writeln(e);
}
```

`Document.save()` は返り値がないメソッド

これは .jsx です