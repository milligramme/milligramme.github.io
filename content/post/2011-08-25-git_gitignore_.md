+++
title = ".gitignore を共有したい"
date = "2011-08-25T00:00:00+09:00"
tags = ["development", "git"]
+++
Git を使っていて必ずといっていいほど、.gitignoreに .DS_Store を追加しているのをそろそろ卒業したい。調べてみると ~/.gitconfig に 

```
$ git config --global core.excludesfile ~/.gitignore
```
的な内容を書き込めば、~/.gitignore に書き込んだ設定を共有できるらしい。

```bash
#.gitignore
[user]
  name = milligramme
  email = milligramme.cc@gmail.com
[color]
  ui = auto
[core]
  excludesfile = ~/.gitignore
```

これで、$ git init 直後に .DS_Store は除外対象になってる
で、ちょっとだけハマったのが、パス。$HOME や `whoami` だと有効にならなかった。

excludesfile = $HOME/.gitignore ... 効かない
excludesfile = /Users/`whoami`/.gitignore ... 効かない
excludesfile = ~/.gitignore ... 効く
excludesfile = /Users/gdansk/.gitignore ... 効く(コマンドライン↓で設定したのは全てこれになる)

```
$ git confing --global core.excludesfile $HOME/.gitignore
$ git confing --global core.excludesfile /Users/`whoami`/.gitignore
$ git confing --global core.excludesfile ~/.gitignore
```

### 参考にした
- [.gitconfigに設定してるaliasなどのまとめ - ゆろよろ日記](http://d.hatena.ne.jp/yuroyoro/20101008/1286531851)
- [Help.GitHub - Ignore files](http://help.github.com/ignore-files/)
- [github/gitignore - GitHub](https://github.com/github/gitignore)

