+++
title = "IllustratorでCADデータを開く"
date = "2014-02-25T00:00:00+09:00"
tags = ["extendscript", "illustrator"]
+++

```js
var open_opt = app.prefereneces.AutoCADFileOptions;
with(open_opt) {
  // property = value;
  // property = value;
  // property = value;
  // property = value;
}
var doc = app.open("/path/to/dwg_file.dwg", DocumentColorSpace.CMYK, open_opt);
```


AI.jsx で CADデータ開くとき、 `open()` 第3引数のオプションを指定するとおこられるが、指定しないとオプションダイアログがでてしまう。 AutoCADFileOptions を設定してオプションダイアログをスキップするには、第3引数を設定せず

```js
app.userInteractionLevel = UserInteractionLevel.DONTDISPLAYALERTS;
```

を挟んで無理やり出さないでやるらしい。。。

第2引数の DocumentColorSpace は指定しても無視され、 RGB から変更できない。CMYK に変更するには、

```js
var new_doc = app.documents.add(DocumentColorSpace.CMYK);
```

してCMYK書類に複製すればいいらしい。。。

または、RGB→CMYK変換のアクションを使えばいいらしい。。。

いずれにしても AI.jsx ひどい


### 参考にした
- [Adobe Community: First time scripter - Advice please?](http://forums.adobe.com/message/3930842#3930842)
- [Adobe Community: Convert RGB to CMYK  Illustrator CS4 js](http://forums.adobe.com/message/2921358#2921358) ←さすがに copy() paste() は使わない。