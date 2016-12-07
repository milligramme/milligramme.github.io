+++
title = "RVMのアップデートとGemを引っ越しメモ"
date = "2011-01-21T00:00:00+09:00"
tags = ["ruby", "rvm", "textmate"]

outdated = true
+++
先日、RVMの脆弱性のニュース

[「Ruby Version Manager」にエスケープシーケンスインジェクションの脆弱性 - CNET Japan](http://japan.cnet.com/news/business/20425220/) 

が発表されていたので、RVMのアップデート（v 1.1.6 => v 1.2.2）をしたら、TextMateのWrapperが効かなくなった（1.9.2から1.8.7に切り替わらない）ので調べてみました。

どうやら、RVMのRuby 1.9.2のデフォルト？がruby-1.9.2-p0 から ruby-1.9.2-p136に変わったのが関係してそう

```bash
$ rvm use 1.9.2
warn: ruby ruby-1.9.2-p136 is not installed.
To install do: 'rvm install ruby-1.9.2-p136'
```

となる、インストール。

Gemの移行をどうやるんだろうと調べていたら、ちょうど

[rvmで1.9.2-p0から1.9.2-p136に乗り換える。rvm aliasも使ってみた。 - 会長@腹部日記(2010-12-26)](http://www.tamoot.net/d/20101226.html) 

というエントリーがあったので試してみました。（rvm aliasは必要なかったので試してないです。）

    rvm migrate [古いRuby] [新しいRuby]

とやるらしい

RVMで1.9.2を使用したくても怒られるので（1.9.2-p0なら大丈夫）

1.9.2-p136をインストールします。

```
$ rvm use 1.9.2
warn: ruby ruby-1.9.2-p136 is not installed.
To install do: 'rvm install ruby-1.9.2-p136'

$ rvm list

rvm rubies

   ruby-1.8.7-p302 [ x86_64 ]
   ruby-1.9.2-p0 [ x86_64 ]

$ rvm install 1.9.2-p136 --with-readline-dir=$rvm_path/usr
/Users/gdansk/.rvm/rubies/ruby-1.9.2-p136, this may take a while depending on your cpu(s)...

ruby-1.9.2-p136 - #fetching 
ruby-1.9.2-p136 - #downloading ruby-1.9.2-p136, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 8612k  100 8612k    0     0  3137k      0  0:00:02  0:00:02 --:--:-- 4037k
ruby-1.9.2-p136 - #extracting ruby-1.9.2-p136 to /Users/gdansk/.rvm/src/ruby-1.9.2-p136
ruby-1.9.2-p136 - #extracted to /Users/gdansk/.rvm/src/ruby-1.9.2-p136
ruby-1.9.2-p136 - #configuring 
ruby-1.9.2-p136 - #compiling 
ruby-1.9.2-p136 - #installing 
ruby-1.9.2-p136 - updating #rubygems for /Users/gdansk/.rvm/gems/ruby-1.9.2-p136@global
ruby-1.9.2-p136 - updating #rubygems for /Users/gdansk/.rvm/gems/ruby-1.9.2-p136
ruby-1.9.2-p136 - adjusting #shebangs for (gem).
ruby-1.9.2-p136 - #importing default gemsets (/Users/gdansk/.rvm/gemsets/)
Install of ruby-1.9.2-p136 - #complete 

$ rvm list

rvm rubies

   ruby-1.8.7-p302 [ x86_64 ]
   ruby-1.9.2-p0 [ x86_64 ]
=> ruby-1.9.2-p136 [ x86_64 ]
```

1.9.2-p136がインストールされました、1.9.2-p0のGemを1.9.2-p136に移動させます。

途中で、Gemの上書きしますか？エイリアスの移動をしますか？Wrapperの移動しますか？古いRUbyを削除しますか？など訊かれるので適宜答えて（Y/n）完了

```
$ rvm migrate 1.9.2-p0 1.9.2-p136
Are you sure you wish to MOVE gems from ruby-1.9.2-p0 to ruby-1.9.2-p136?
This will overwrite existing gems in ruby-1.9.2-p136 and remove them from ruby-1.9.2-p0 (Y/n): Y
Moving gemsets...
Moving ruby-1.9.2-p0 to ruby-1.9.2-p136@ruby-1.9.2-p0
Making gemset ruby-1.9.2-p136@ruby-1.9.2-p0 pristine.
Moving ruby-1.9.2-p0@global to ruby-1.9.2-p136@global
Making gemset ruby-1.9.2-p136@global pristine.
Do you wish to move over aliases? (Y/n): Y
Do you wish to move over wrappers? (Y/n): Y
Do you also wish to completely remove ruby-1.9.2-p0 (inc. archive)? (Y/n): Y
Successfully migrated ruby-1.9.2-p0 to ruby-1.9.2-p136
```

では、TextMateのRunコマンド（Command+R）で使うRubyを切り替えてみます


```
$ rvm wrapper 1.8.7 textmate
```

よし、1.8.7に切り替わっ、た…！？

![/images/2011/01/why_system_ruby187.png](/images/2011/01/why_system_ruby187.png)

System Ruby !?

```
$ rvm use 1.8.7
warn: ruby ruby-1.8.7-p330 is not installed.
To install do: 'rvm install ruby-1.8.7-p330'
```

1.8.7も 1.8.7-p302から1.8.7-p330がデフォルトに変わったようですね。

同様の作業をRuby1.8.7に対しても行います。（省略）

改めて

```bash
$ rvm wrapper 1.8.7 textmate
```

![/images/2011/01/wrapper_ruby187_330.png](/images/2011/01/wrapper_ruby187_330.png)

よし大丈夫。うん多分。