+++
tags = ["electron","nodejs"
]
date = "2016-11-25T12:37:35+09:00"
draft = false
outdated = false
title = "古いElectron.appをアップデート"
description = ""

+++

古いElectron.appをいじる必要があり

nodejs v4.4.5 -> v6.3.0

electron v0.36.7 -> v1.4.8

にアップデートした。


[npm install electron \- Electron](http://electron.atom.io/blog/2016/08/16/npm-install-electron)

まず、 `electron-prebuilt` がDeprecatedのようなので、 `electron` をインストールしなおす。

```
% npm -g install electron
```

requireまわりが変わっていたので書き換え。

app.js

```diff
-var remote = require('remote');
-var dialog = remote.require('dialog');
-var browserWindow = remote.require('browser-window');
+const electron = require('electron')
+const remote = electron.remote
+const dialog = remote.dialog
+const browserWindow = remote.BrowserWindow
```

main.js

```diff
-var app = require('app');
-var BrowserWindow = require('browser-window');
-var Menu = require('menu');
-require('crash-reporter').start({
+const electron = require("electron")
+const app = electron.app
+const BrowserWindow = electron.BrowserWindow
+const Menu = electron.Menu
+const crashReporter = electron.crashReporter
+crashReporter.start({
   productName: 'sample.app',
   companyName: 'COMPANY',
   submitURL: 'https://your-domain.com/url-to-submit',
   autoSubmit: false
-});
+})

```

とりあえず、これでそのまま動いた。

```
% electron .
```



