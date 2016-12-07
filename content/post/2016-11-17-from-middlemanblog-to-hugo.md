+++
title = "middleman-blogからhugoに移行"
date = "2016-11-17T09:23:00+09:00"
tags = ["hugo", "middleman"]

outdated = false
+++

middleman blogからhugo blogへの移行

とりあえず、 [Hugo Theme: Blackburn](http://themes.gohugo.io/blackburn/) 

middleman/postをhugo/content/post

middleman/imagesをhugo/static/images

に移し

記事内のfrontmatterはそのまま `yaml` でもいけるけど `toml` に変換

config.tomlを作成して

`hugo` コマンドでサイト生成

新規記事は

```
$ hugo new post /PATH/TO/POST.md
```

で生成



"2006-01-02T03:45:00+09:00"

Macの `date` コマンドが古くて `date "+%FT%:z"`