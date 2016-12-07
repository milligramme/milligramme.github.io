+++
title = "gitで空コミットする"
date = "2015-01-28T00:00:00+09:00"
tags = ["git"]
+++

cedar-14 スタックにアップグレードする方法をさがしてて

[Migrating to the Celadon Cedar-14 Stack \| Heroku Dev Center](https://devcenter.heroku.com/articles/cedar-14-migration)

で git で空コミットする方法をみつけた

```
$ git commit --allow-empty -m "Upgrading to Cedar-14"
```

とかすると変更内容なしでもコミットできる
