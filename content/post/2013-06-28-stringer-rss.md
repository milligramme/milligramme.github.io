+++
title = "Rssリーダには Stringer を使う（つもり）"
date = "2013-06-28T00:00:00+09:00"
tags = ["rss", "heroku", "ruby"]
+++

![2013 06 28 Google Reader Will Die](/images/2013-06-28-google-reader-will-die.png)

2013年7月1日で Google Reader が終了するにあたって、 [Stringer](https://github.com/swanson/stringer) を [Heroku](https://www.heroku.com/) にあげて試用してみている。7月以降も、たぶんそのまま使うつもり。

設置方法は READMEの [Installation](https://github.com/swanson/stringer#installation) を見たらだいたいわかると思う。

### 機能・仕様
* アンチソーシャル (F\*c\*book?, Tw!tt3r? なにそれ)
* 自分で Heroku などにアプして使う
* キーボードショートカット
* スター ★
* 多言語対応 (2013-06-28現在 :en + 14言語) ( `$ heroku config:set LOCALE=ja` で日本語化 )
* Fever API 対応


あと、タイムゾーンを設定しないと :UTC になっているっぽいので、 `$ heroku config:set TZ=Asia/Tokyo` するといい。

#### 2013-07-03追記
2013-07-03現在、Herokuで新たにアプリを作成したときrubyのバージョンが `2.0.0` になっています（自分が作ったときはまだ `1.9.2` でした）。

Gemfileで  `ruby '1.9.3'` とすれば `1.9` 環境でも動作できますが `2.0` でも問題なさそうです。なんとなくサクサク動く気がする..
