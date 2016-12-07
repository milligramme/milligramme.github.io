+++
title = "OSX.jsxでFile.openDialog（）の拡張子フィルター"
date = "2014-09-16T00:00:00+09:00"
tags = ["extendscript"]
+++

windowsだと `File.openDialog()` でファイルを拡張子でフィルターできるけど

```js
File.openDialog("*.jpg");
```

osxだとできないので function わたせばいい

```js
#target "photoshop"

// jpgとフォルダ以外選択できないようにする
var jpg = File.openDialog('JPG!',function (fi) {
  return (fi.constructor.name === 'Folder' || /.+\.jpe?g/i.test(fi.name));
});

$.writeln(jpg);
```

おまけ

`getFile()` で拡張子以外にもフィルター

```js
#target "photoshop"

var fo = Folder.selectDialog();
var jpgs = fo === null ? [] : fo.getFiles(function (fi) {
  var _name = /.+\.jpe?g/i.test(fi.name);
  var _size = fi.length > 1000; // byte
  var _modified = fi.modified.getTime() > new Date(2013,9,16,0,0).getTime();
  return (_name && _size && _modified)
})

$.writeln(jpgs);
// => ["/path/to/a.jpeg", "/path/to/b.jpg"]
```
