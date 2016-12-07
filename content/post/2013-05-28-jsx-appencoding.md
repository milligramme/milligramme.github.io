+++
title = "jsx で $.appEncoding を使用する"
date = "2013-05-28T00:00:00+09:00"
tags = ["extendscript"]
+++

jsxでアプリのデフォルトエンコーディングは `cp932` になっているので
File書出し・読込みのときの無指定だと `cp932` になる。

ユニコードで処理したいとき、いちいち encoding 指定しないといけない。

`$.appEncoding = "UTF-8";` などと宣言するとアプリが終了するまで保持し続けてくれる。


```js
#target "InDesign"

var content_sjis = "むかし国立に吉野屋ＵＳＡがあった。";
var content      = "むかし国立に𠮷野屋ＵＳＡがあった。";

var file_default1        = new File("~/Desktop/default_enc1.txt");
var file_default2        = new File("~/Desktop/default_enc2.txt");
var file_utf8            = new File("~/Desktop/utf8.txt");
var file_use_appencoding = new File("~/Desktop/use_appencoding.txt");


var write_with_encoding = function (file, content, encoding) {
  var encoding = encoding || undefined;
  
  // ファイルを開く前にエンコーディングを指定する
  // 指定しないと通常 cp932 になるので utf-8 などを指定
  file.encoding = encoding;
  if (file.open("w")) {
    $.writeln(file.name + ": " + file.encoding);
    file.write(content)
  }
  file.close();
}

if (new File($.fileName).name==$.stack.replace(/[\[\]\n]/g,"")) {
  
  $.writeln("アプリのデフォルトエンコーディングは" + $.appEncoding);
  write_with_encoding(file_default1, content_sjis, undefined);
  write_with_encoding(file_default2, content, undefined); // 書き出されない

  // Fileにエンコーディング設定
  write_with_encoding(file_utf8, content, "UTF-8");

  // アプリでエンコーディング設定、Fileは無指定
  $.appEncoding = "UTF-8";
  write_with_encoding(file_use_appencoding, content, undefined);
  $.appEncoding = "cp932";
}

// => アプリのデフォルトエンコーディングはCP932
// => default_enc1.txt: CP932
// => default_enc2.txt: CP932
// => utf8.txt: UTF-8
// => use_appencoding.txt: UTF-8
```