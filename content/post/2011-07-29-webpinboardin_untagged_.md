+++
title = "Pinboard.in の untagged をまとめて削除"
date = "2011-07-29T00:00:00+09:00"
tags = ["api", "ruby", "cli"]

outdated = true
+++

SBM には delicious から乗り換えて pinboard.in を使っています。 

今回は誤操作で大量のリンクをブックマーク追加してしまったのを削除したときのメモです。

Pinboardの設定項目 settings > twitter

- Twitterでつぶやいたツイート内のリンクを追加
- Favしたツイート内のリンクを追加

を有効にして試したところ

![/images/2011/07/pinboard_twitter.png](/images/2011/07/pinboard_twitter.png)

適用後のツイートから追加されると思いきや、過去にさかのぼってリンクが大量に追加され、気付くと1400件ほどのリンク（soundtracking や twipic のとか含む）が追加されてしまいました (T_T)。

ブックマークをみてみると

![/images/2011/07/from_twitter.png](/images/2011/07/from_twitter.png)

from:twitter　from:twitter_favs

というタグ的なものはついているのですが、タグとは違い、抽出して削除するにもそれらしき要素もなく api もそれに対応した項目がないようです。

考えたあげく、タグをつけてないブックマークとほぼ一致することに気付いたのですが、untagged も開発中のようで api 対応してない(T_T)

で、こんな感じで考えてみた

<del datetime="2011-07-29T03:48:48+00:00">
  XML書き出し→タグがないブックマークをNokogiriで削除→ブックマークを全削除して再度読み込み
</del>

XML書き出し→タグがないブックマークをNokogiriで抽出（削除urlファイル）→shellscriptとして削除urlファイルを実行

Pinboard の APIは
 [Pinboard API (v1) Documentation](http://pinboard.in/api/) 

とりあえず全部取得

```bash
curl https://USER_ACCOUNT:PASSWORD@api.pinboard.in/v1/posts/all > pinboard.xml
```

をローカルに xml書出し（open-urlでhttpsがうまくできなかったので、次回トライ）


```ruby
# -*- coding: utf-8 -*-
require "nokogiri"
require "open-uri"
require "cgi"

user    = "USER_ACCOUNT"
pw      = "PASSWORD"
del_url = "https://"+user+":"+pw+"@api.pinboard.in/v1/posts/delete"

del_url_arr = []
doc = Nokogiri::XML( open("./pinboard.xml") )
doc.xpath('//post[@tag=""]').each do |n|
  del_url_arr << n.attributes['href'].value
end

file = open("pinboard_delete.sh","w")
file.write("#!/usr/bin/env bash\n")
del_url_arr.each do |del|
  durl = CGI.escape(del)
  file.write("curl " + del_url + "?url=#{durl}\n")
end
file.close
```

Nokogiriでtag=""のurlを抜き出し

「curl で 削除リクエストを url の数だけ出す」 ShellScriptを書き出す↓

```bash
% curl https://USER_ACCOUNT:PASSWORD@api.pinboard.in/v1/posts/delete?消したいURL
```

ShellScriptを実行して削除

![/images/2011/07/no_untagged.png](/images/2011/07/no_untagged.png)

ものすごくかっこわるいので api が更新されたらまた試してみたい。