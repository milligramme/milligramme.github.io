+++
title = "git log メモ"
date = "2012-02-08T00:00:00+09:00"
tags = ["git"]
+++

### 基本の `git log`

```
git log --pretty=medium #と同等

git log --pretty=oneline
git log --pretty=short
git log --pretty=medium
git log --pretty=full
git log --pretty=fuller
git log --pretty=email
git log --pretty=raw
```

### ミニマム `git log oneline`

```
git log --pretty=oneline --abbrev=commit #と同等
```

### diffstat付 `git log stat` 下に行くほど簡略

```
git log --stat
git log --numstat
git log --shortstat
```

### ブランチのマージなどを表示 `git log graph`

```
git log --graph
git log --graph --oneline
```

### ブランチ・タグなどを表示 `git log decorate`

```
git log --decorate
git log --decorate --oneline
```

### 好きなフォーマットで出力 `git log pretty=format:<string>`

```bash
git log --pretty=format:"%h was%Cred%an-%Cgreen%s-%Cblue%d%m"
git log --pretty=format:"%h was%Cred%an-%Cgreen%s-%Cblue%d%m"
git log --pretty=format:"%h %Cgreen %cd %Cred%cr %ct %ci"

%H: commit hash
%h: abbreviated commit hash #省略型
%an: author name
%ad: author date (format respects --date= option)

%aD: author date, RFC2822 style
git log --pretty=format:"%aD"
git log --pretty=format:"%ad" --date=rfc　 #と同等

%ar: author date, relative #相対的（n日前とか）
git log --pretty=format:"%ar"
git log --pretty=format:"%ad" --date=relative　 #と同等

%at: author date, UNIX timestamp
git log --pretty=format:"%at"
1324029447
git log --pretty=format:"%ad" --date=raw
1324029447 +0900

%ai: author date, ISO 8601 format  #年月日 時分秒
git log --pretty=format:"%ai"
git log --pretty=format:"%ad" --date=iso #と同等
git log --pretty=format:"%ad" --date=short #年月日だけ

%d: ref names #ref, branch, tagを表示
```

### 色を設定

```bash
%Cred: switch color to red
%Cgreen: switch color to green
%Cblue: switch color to blue
%Creset: reset color
%C(...): ["Black","Red","Green","Yellow","Blue","Magenta","Cyan","White"]
git log --pretty=format:"%h %C(Yellow)%d %C(Cyan)%aD"
```

### おまけ

割と好きな表示

```
$ git log --oneline --decorate --stat
```

といいつつ、普段は tig を使ってる

```
$ brew install tig
```
