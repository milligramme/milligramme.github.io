+++
title = "Rfm Code Reading #10にいってきました"
date = "2011-02-18T00:00:00+09:00"
tags = ["event", "filemaker", "rails"]

outdated = true
+++

さる 2011-02-11 雪の降る中  [Rfm Code Reading #10 開催のお知らせ - FAMLog in Hatena](http://d.hatena.ne.jp/matsuo_atsushi/20110203/p1)  に参加してきました。

**Rfm** は Rubyアプリから FileMaker Server に接続するためのAPIで、v1.4からLardawgeさん（読み方がわからない）にメンテナーが変わっています。=>  [Lardawge-Rfm](https://github.com/lardawge/rfm) 

v1.4のソースを見るに当たってREADMEに

> Master branch is unstable at this point because I am pushing major changes. The stable branch is v1.4.

とあり、master ブランチが開発用のHEADになっているので tag を **v1.4.1.2**に切り替えて見る必要があるようです。

Lardawge版のrfmのソースを見ながら、変わったところの確認。

### Rfm1.0 と Lardawge-Rfm 1.4 のファイル構成の違い

左がLardawge版、右が旧SixFriedRice版

![/images/2011/02/compare_rfm.png](/images/2011/02/compare_rfm.png)


- 1クラス1ファイルのように分割、再構成されている。旧：rfm_command.rbは
    - server.rb
    - layout.rb
    - dababase.rb
    - metadetaフォルダ
    と分かれます
- テストが Test::UtilitiesからRSpec (1.3.1)に
- XMLの処理が REXML から Nokogiri に
- SSLサポート


- インストールの仕方
- RSpecでのテスト方法
- specコマンドとオプションについて

についてRubyまわりRailsまわりFileMakerまわりの雑談を交えて、触れていきました。

次回につづく

### 主催者のブログエントリー
- [Rfm 1.4をインストールするには - FAMLog in Hatena](http://d.hatena.ne.jp/matsuo_atsushi/20110209/p1) 
- [Rfm 1.4でテストを実行するには - FAMLog in Hatena](http://d.hatena.ne.jp/matsuo_atsushi/20110211/p1) 