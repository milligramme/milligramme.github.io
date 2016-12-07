+++
title = "Alternative InDesign API documentation"
date = "2013-10-10T00:00:00+09:00"
tags = ["nodejs", "indesign", "api"]

outdated = true
+++

[Adobe Community: Alternative InDesign API documentation](http://forums.adobe.com/thread/1312045)

InDesignのOMV(Object Model Viewer)にかわるドキュメントをローカルに生成できるみたいのでためした

### 必要なもの

Node.jsのインストールとInDesignのOMVのxmlファイルが必要


### とりあえず git clone

```bash
git clone https://github.com/yearbookmachine/indesign-api-documentation.git
$ cd indesign-api-documentation.git
```

### 該当ファイルをコピしてきて一部修正

場所がわからなかったら `mdfind` コマンドで探した方が手っ取り早い

```bash
$ cp ~/Library/Preferences/ExtendScript\ Toolkit/3.5/omv\$indesign-7.0\$7.0.xml  ./xml/indesign.xml

$ mdfind -name javascript.xml
$ /Library/Application Support/Adobe/Scripting Dictionaries CS5/CommonFiles/javascript.xml
$ cp -v /Library/Application\ Support/Adobe/Scripting\ Dictionaries\ CS5/CommonFiles/javascript.xml ./xml
/Library/Application Support/Adobe/Scripting Dictionaries CS5/CommonFiles/javascript.xml -> ./xml/javascript.xml

$ mdfind -name scriptui.xml
$ /Library/Application Support/Adobe/Scripting Dictionaries CS5/CommonFiles/scriptui.xml
$ cp -v /Library/Application\ Support/Adobe/Scripting\ Dictionaries\ CS5/CommonFiles/scriptui.xml ./xml
/Library/Application Support/Adobe/Scripting Dictionaries CS5/CommonFiles/scriptui.xml -> ./xml/scriptui.xml

$ ls ./xml
indesign.xml   javascript.xml scriptui.xml
```

javascript.xmlとscriptui.xmlの頭2行を `<dictionary engine="">` にする

```bash
$ npm install
$ node build.js
(1/3) =============================
(2/3) =============================
(3/3) =============================
```

./source に json 群ができる

### httpサーバたてる

[wridgers/zapp](https://github.com/wridgers/zapp)

```bash
$ npm install -g zapp
$ cd docs
$ zapp
```

http://localhost:8080/ で確認できる


### スタティックなHTMLにする

```bash
$ npm install -g jade markdown snockets-cli less
$ ./generate
Checking for required tools...
Checking for required source files...
Removing stale baked goods...
Generating documentation source files...
(1/3) =============================
(2/3) =============================
(3/3) =============================
Baking docs/ folder into html/...

  rendered html/index.html


  rendered html/templates/index.html
  rendered html/templates/search.html
  rendered html/templates/class-detail.html

Compressing docs into zip file...
Done. Enjoy some freshly baked docs. :)
```

./html に html ファイルができているはず