+++
title = "OSX YosemiteにAdobe CS3"
date = "2015-04-20T00:00:00+09:00"
tags = ["osx"]
+++

こどもの遊び用の MacBook Airを Marvericks から Yosemite 10.10.3 にしたときのトラブル。

JAVA Update 後、Adobe CS3 Appを起動するも

「ライセンシングがなんちゃんら」とでて起動しない。

[Mac OS X 10\.6 アップグレード後に「ライセンシングが動作していません」エラーが表示される](https://helpx.adobe.com/jp/x-productkb/global/236044.html)

を参考にして

[Adobe Licensing Repair Tool](http://www.adobe.com/jp/support/contact/licensing.html)

で修復する。

![2015 04 20 Repair Tool](/images/2015-04-20-repair-tool.png)

- ライセンス修復ツールが ppc アプリなので起動しない
- コマンドラインから LicenseRecover.py を実行
- SNを再入力
- InDesignとPhotoshopは起動
- IllustratorとBridgeで 「VersionCue CS3 Framework なんちゃら」がでて起動しない

[Fatal error \- missing component \(VersionCue\.fra\.\.\. \| Adobe Community](https://forums.adobe.com/thread/425803)

とっくにサービス終了してたVersionCue関係のファイルを削除したのが原因。

再インストール面倒なので、バックアップから該当ファイルを戻したら起動するようになった。