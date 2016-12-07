+++
title = "Processing2 がApplicationsフォルダにあると起動しない"
date = "2014-04-25T00:00:00+09:00"
tags = ["processing", "osx"]

outdated = true
+++

久々に Processing を使おうと思ったら

![2014 04 25 Processing2 In Applications](/images/2014-04-25_processing2_in_applications.png)

となって起動しない。(version 2.1.1)

最新の version 2.1.2 を落としてきて、zip解凍して起動すると起動した!?

でも /Applications フォルダに移してみたらまた同じアラートがでる

[Getting Started \ Processing.org](https://www.processing.org/tutorials/gettingstarted/)

    The Mac OS X version is also a .zip file. Double-click it and drag the Processing icon to the Applications folder. If you're using someone else's machine and can't modify the Applications folder, just drag the application to the desktop. Then double-click the Processing icon to start.

動かなくなる理由はわからずじまい

---

あと、だいぶ前(version 2.0.8) から use external editor 機能が廃止され、 `processing-java` コマンドを使うようになったらしい

osxだと別途ダウンロードして使うらしいのですが、gglって探してもどこからダウンロードするのかが見つからない

ふとメニューを見ると

![2014 04 25 Install Processing Java](/images/2014-04-25_install-processing-java.png)

all user か home (/Users/xxx/ )にインストールするかきいてくるので、all user (YES) でインストール

![2014 04 25 Where To Install](/images/2014-04-25_where-to-install.png)

```
% which processing-java
/usr/bin/processing-java
```

シェルスクリプトがインストールされるので、みてみる、アプリのパスの変えたらうごかない..

```bash
#!/bin/sh
cd "/Users/xxxxx/Processing.app/Contents/Java" && /Users/xxxxx/Processing.app/Contents/PlugIns/jdk1.7.0_51.jdk/Contents/Home/jre/bin/java -cp "pde.jar:antlr.jar:jna.jar:ant.jar:ant-launcher.jar:org-netbeans-swing-outline.jar:com.ibm.icu_4.4.2.v20110823.jar:jdi.jar:jdimodel.jar:org.eclipse.osgi_3.8.1.v20120830-144521.jar:core/library/core.jar" processing.mode.java.Commander "$@"
```

この書き方だと動かない

```
% cd ~/Documents/Processing
% processing-java --run --sketch=sketch_140425d --output=sketch_140425d/out
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
sketch_140425d does not exist.
```

絶対パスか、カレントディレクトリを含める

```
% processing-java --run --sketch=~/Documents/Processing/sketch_140425d/ --output=~/Documents/Processing/sketch_140425d/out
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Listening for transport dt_socket at address: 8140
Finished.
% processing-java --run --sketch=./sketch_140425d/ --output=./sketch_140425d/out
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Listening for transport dt_socket at address: 8544
Finished.
```


### 参考にした
- [Processing を好きなエディタでコーディングしてコマンドラインから実行する - 雑念日記](http://hoshi-sano.hatenablog.com/entry/2013/08/01/213236)