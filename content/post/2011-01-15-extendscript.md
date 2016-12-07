+++
title = "共通点を探す旅（保存して閉じる編）"
date = "2011-01-15T00:00:00+09:00"
tags = ["extendscript", "illustrator", "indesign", "photoshop"]
+++
InDesign, Illustrator, Photoshop共通で使える、大量に開いた未保存ドキュメントを全部保存して閉じるスクリプト

以前に書いた「保存しないで閉じるスクリプト」 [[ExtendScript]共通点を探す旅（保存せずに閉じる編）](http://www.milligramme.cc/wp/archives/493) は割とよく使うのですが、その逆です。
一度ファイルとして保存されたもの（上書き保存）は、そのまま保存して閉じていきます。一度も保存したことのないドキュメントはその都度保存ダイアログがでます。保存をキャンセルするとそこで処理が中断されます。

```js

/*
Close all documents with saving
for Photoshop, Illustrator, InDesign
*/ 
switch(app.name){
  case "Adobe Photoshop":
  case "Adobe Illustrator":
  while (app.documents.length > 0){
    try {
      app.activeDocument.close(SaveOptions.SAVECHANGES);
      // if document has not saved yet, saving dialog appear
    }
    catch(e){
      // if saving dialog was canceled
      alert("saving document was canceled by user");
      break;
    }
  }
  break;

  case "Adobe InDesign":
  while (app.documents.length > 0){
    try {
      app.activeDocument.close(SaveOptions.yes);
      // if document has not saved yet, saving dialog appear
      }
    catch(e){
      // if saving dialog was canceled
      alert("Saving document was canceled by user");
      break;
    }
  }; 
  break;
}
```