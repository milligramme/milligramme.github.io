+++
title = "内蔵エディタのかわりに外部エディタをつかうときの設定"
date = "2012-07-23T00:00:00+09:00"
tags = ["processing", "textmate"]

outdated = true
+++
![use_internal](/images/2012/07/use_internal.png)

[Processing](http://processing.org/ "Processing.org") を書くとき、

内蔵エディタがアレ（日本語通らない、コード補完しない もろもろ）なので、TextMateにProcessing用のバンドルをいれて利用している。

ただ、いままで初期設定の `Preferences > Use external editor` というのにチェックしてなくて、

ちゃんと外部エディタとして機能してなかったっぽい（Processingエディタの内容を上書きせずに実行しちゃうとか）のでメモ。

![use_extarnal_check](/images/2012/07/use_extarnal_check.png)

Preferences > Use external editor

上記のようにチェックすると Processingエディタのバックグラウンドが白からブルーグレーっぽく変わって、テキスト入力を受け付けなくなる

こうすることで
TextMateで書く、保存、実行 => Processingのエディタの更新 => コンパイル => jarの実行
がスムースに動作する

![use_textmate](/images/2012/07/use_textmate.png)

![use_extarnal](/images/2012/07/use_extarnal1.png)

![exec_jar](/images/2012/07/exec_jar.png)

Processing.tmbundle は[コレ](https://github.com/textmate/processing.tmbundle)をフォークして、使いながらスニペットを足していってる

[processing.tmbundle](https://github.com/milligramme/processing.tmbundle) 