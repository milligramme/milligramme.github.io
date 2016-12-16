+++
title = "InDesign.jsxで配置プログレスを表示しない"
date = "2016-12-01T17:33:54+09:00"
description = ""
tags = ["indesign", "extendscript"
]
categories = [
]
draft = false
outdated = false

+++

InDesignで大量の画像読み込み＆配置処理があるときに、
読み込みダイアログがその画像ごとに、表示・非表示と繰り返されるのが鬱陶しい。

```js
//@target indesign
var F = Folder(data_folder_path);
var data = F.getFiles(function (file) {return /.+\.pdf$/.test(file.name)});
var doc = app.open(File(idml_path);
for (var i=0, len=data.length; i < len ; i++) {
  doc.pages[0].place(File(data[i]));
};
```

こういうの

![2016 12 16 Image Progress](/images/2016-12-16-image-progress.png)


配置する前に、`UserInteractionLevels.INTERACT_WITH_ALERTS` でアラートを抑止するとだまって配置してくれる。

処理が終わったら、`UserInteractionLevels.INTERACT_WITH_ALL` で戻す。

```diff
--- old
+++ new
@@ -2,6 +2,11 @@
 var F = Folder(data_folder_path);
 var data = F.getFiles(function (file) {return /.+\.pdf$/.test(file.name)});
 var doc = app.open(File(idml_path);
+
+app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALERTS;
+
 for (var i=0, len=data.length; i < len ; i++) {
   doc.pages[0].place(File(data[i]));
 };
+
+app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;
```