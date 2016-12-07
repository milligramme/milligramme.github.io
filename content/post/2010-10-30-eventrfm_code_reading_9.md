+++
title = "Rfm Code Reading #9にいってきました"
date = "2010-10-30T00:00:00+09:00"
tags = ["event", "filemaker", "ruby"]
+++
台風上陸間近な中、 [Rfm Code Reading #9 ](http://d.hatena.ne.jp/matsuo_atsushi/20101019/p1) というソースコード読書会に参加してきました。

[Rfm](http://sixfriedrice.com/wp/products/rfm/)  （日本語訳は [こちら](http://www.famlog.jp/rfm/) ）はRubyのライブラリーで、Rubyスクリプト や Railsアプリから FileMaker Server に接続するための API です。

今回は主催者の松尾さんの都合で予定外のところを「FileMaker ユーザーグループ 全国合同ミーティング 2010」前にあわせて急遽開催となり、総集編的な位置づけでした。

Rfm の概要説明と今までの活動などをスライド資料で振り返るといったもの。

実際のソースコードに触れるというのは今回ほとんどなかったのですが、次回は 公式 rfm1.0 とメンテナーがかわった rfm1.4 の差分を追うといった内容のようです。

次回参加するにあたってのワンクッション予習をおけた感じで行ってよかった。Ruby勉強しておこう。

いい意味でゆるい感じの脱線気味な勉強会で、いろいろ気になるキーワードがあったのでメモ

- [Phusion Passenger](http://www.modrails.com/) （Apacheモジュール）
- [rvm](http://rvm.beginrescueend.com/) （Ruby バージョン管理）
- [lardawge's rfm](http://github.com/lardawge/rfm) （新しいメンテナー版のrfm）
- [mech's fm_store ](http://github.com/mech/fm_store)  （）
- [FMCakeMix](http://www.beezwax.net/solutions/FMCakeMix)  (CakePHP用 FileMakerドライバー)
- [radiantcms.org](http://radiantcms.org/)  （Ruby製CMS）
- [enote for community](http://www.enote.jp/)  （Ruby製グループウェア）
- [Lokka](http://lokka.org/)  （Ruby製CMS）

気になったコマンド

- Rails debug <code>gem install ruby-debug</code>
- Open current directory in Finder <code>open .</code>
