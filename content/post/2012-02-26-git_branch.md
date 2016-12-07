+++
title = "マージされてないbranchを調べる"
date = "2012-02-26T00:00:00+09:00"
tags = ["git"]
+++

gitでまだマージされてないbranchを調べたい

全てのブランチ

```
$ git branch
  imageupload
* master
  search
  sorcery
  zip
```

マージ済のブランチ

```
$ git branch --merged
* master
  search
  sorcery
  zip
```

未マージのブランチ

```
$ git branch --no-merged
  imageupload
```
