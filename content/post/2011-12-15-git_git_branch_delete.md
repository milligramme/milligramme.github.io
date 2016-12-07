+++
title = "git branch -dと-D のちがいメモ"
date = "2011-12-15T00:00:00+09:00"
tags = ["git"]
+++
Gitでブランチの削除にずっと -D オプションを使っていた。
help によると

```
$ git branch --help

<前略>
OPTIONS
      -d, --delete
           Delete a branch. The branch must be fully merged in its upstream branch, or in
           HEAD if no upstream was set with --track or --set-upstream.

       -D
           Delete a branch irrespective of its merged status.
```

-d または --delete はマージされたブランチは削除できる

-D は容赦なく（irrespective）ブランチを削除する

master にマージしてない 'notyet' ブランチを -d オプションで削除しようとすると -D を使えと怒られる

```
$ git checkout master
Switched to branch 'master'

$ git branch -d notyet
error: The branch 'notyet' is not fully merged.
If you are sure you want to delete it, run 'git branch -D notyet'.
```

おまけ
リモートのブランチ keshitai を消すには

```
$ git push origin :keshitai
```


...直感的でない
