+++
title = "forkしたearthquake.gemを本家とmergeしたときのメモ"
date = "2012-01-12T00:00:00+09:00"
tags = ["git"]
+++

ふだん、Twitterクライアントに [@jugyo](http://twitter.com/#!/jugyo) さんのつくられた [earthquake.gem](https://github.com/jugyo/earthquake) というCUIのクライアントをつかっています

勉強にとgithubでforkして、ちょこちょこいじってみているのですが、本家で更新があってmergeしてみたのでそのときのメモ

### その前に

普通にインストールすると

```
gem install earthquake
```

で $GEM_HOME にインストールされるのですが、いじるときに面倒だなと思っていたところ、

[@nbqx](http://twitter.com/#!/nbqx)さんにgithubからクローンしたリポジトリなどを参照させるやり方を教わりました。

forkしたリポジトリからclone

```
git clone git@github.com:milligramme/earthquake.git
```

/Users/gdansk/earthquake/ へcloneして .bashrc や .bash_profileに

```bash
# earthquake
alias earthquake="/Users/gdansk/earthquake/bin/earthquake"
```

などと記述して、alias を任意のリポジトリの bin/earthquake に向ける

これで、好きな場所の earthquake を earthquakeコマンドで起動できる

自分はさらに eqコマンドに短縮してます

### ローカルの earthquake リポジトリに本家をマージ

```bash
# リモートブランチ
gdansk: ~/earthquake master ♪ git remote -v
origin  git@github.com:milligramme/earthquake.git (fetch)
origin  git@github.com:milligramme/earthquake.git (push)

# remote に本家を追加して、ブランチを切り替え
gdansk: ~/earthquake master ♪ git remote add jugyo git://github.com/jugyo/earthquake.git
gdansk: ~/earthquake master ♪ git checkout -b jugyo/master
Switched to a new branch 'jugyo/master'

# remoteの追加を確認
gdansk: ~/earthquake jugyo/master ♪ git remote -v
jugyo  git://github.com/jugyo/earthquake.git (fetch)
jugyo  git://github.com/jugyo/earthquake.git (push)
origin  git@github.com:milligramme/earthquake.git (fetch)
origin  git@github.com:milligramme/earthquake.git (push)

# jugyoブランチで本家をpullする
gdansk: ~/earthquake jugyo/master ♪ git pull jugyo master
From git://github.com/jugyo/earthquake
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 lib/earthquake/commands.rb |   10 +++++++++-
 lib/earthquake/output.rb   |   18 ++++++++++++++----
 2 files changed, 23 insertions(+), 5 deletions(-)

# masterに切り替えてmerge
gdansk: ~/earthquake jugyo/master ♪ git checkout master
Switched to branch 'master'
gdansk: ~/earthquake master ♪ git merge -
Updating 83e1c5d..4ba7cef
Fast-forward
 lib/earthquake/commands.rb |   10 +++++++++-
 lib/earthquake/output.rb   |   18 ++++++++++++++----
 2 files changed, 23 insertions(+), 5 deletions(-)
```

これで 本家の :aliasesコマンドが追加されました

⚡ :aliases
{
     ":rt" => ":retweet",
     ":fv" => ":favorite",
    ":ufv" => ":unfavorite"
}

あとは自分のリモートリポジトリに push すれば完了