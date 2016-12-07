+++
title = "共通点を探す旅（保存せずに閉じる編）"
date = "2009-07-28T00:00:00+09:00"
tags = ["extendscript"]
+++
ちょっとずつ違いと共通点を探してみる。
Adobe CS3にて。

```js
/*
全てのドキュメントを保存せずに閉じる
Photoshop, Illustrator, InDesign共用
*/
switch(app.name){
  case "Adobe Photoshop":
  case "Adobe Illustrator":
    while (app.documents.length > 0){
      app.activeDocument.close(SaveOptions.DONOTSAVECHANGES);
    }
    break;
  case "Adobe InDesign":
    while (app.documents.length > 0){
      app.activeDocument.close(SaveOptions.no);
    }
    break;
}
```

InDesignのSave Optionsが違う。