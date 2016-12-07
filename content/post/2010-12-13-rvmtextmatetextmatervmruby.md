+++
title = "TextMateでRVMのRubyを使う"
date = "2010-12-13T00:01:00+09:00"
tags = ["ruby", "rvm", "textmate"]

outdated = true 
+++

普段、テキストエディタにTextMateを使い、ExtendScript でAdobe系のJavaScriptでスクリプトを書きながら時々 Ruby のような生活をしていて、合間にRailsとかSinatraとかをいじってたりします。

TextMateにはRunコマンドというのがあって、TextMate上のコマンド＋ＲでRubyスクリプトをちょちょいと実行できるのですが、デフォルトだとシステムのRubyになってしまい、  [RVM](http://rvm.beginrescueend.com/)  でRubyを管理しているのに、残念な思いをしていました。

でもよくみたら本家にちゃんとやり方が書いてありましたのでメモ。

[RVM: Ruby Version Manager - Textmate Integration with RVM](http://rvm.beginrescueend.com/integration/textmate/) 

### RVMをアップデート

```bash
$ rvm get head

Original installed RVM version:

rvm 1.1.3 by Wayne E. Seguin (wayneeseguin@gmail.com) [http://rvm.beginrescueend.com/]

remote: Counting objects: 59, done.
remote: Compressing objects: 100% (40/40), done.
remote: Total 42 (delta 28), reused 0 (delta 0)
Unpacking objects: 100% (42/42), done.
From http://github.com/wayneeseguin/rvm
 * branch            master     -> FETCH_HEAD
Updating 632af5a..438d119
Fast-forward
 help/gemset                    |   14 +++++++++
 lib/VERSION.yml                |    2 +-
 lib/rvm/environment.rb         |    2 +-
 lib/rvm/shell/shell_wrapper.sh |    2 +-
 pkg/gentoo/rvm-1.1.4.ebuild    |   59 ++++++++++++++++++++++++++++++++++++++++
 scripts/cli                    |    5 ++-
 scripts/gemsets                |   13 ++++++++-
 scripts/irbrc.rb               |    8 +++---
 scripts/selector               |    2 +-
 scripts/tools                  |    4 +-
 10 files changed, 98 insertions(+), 13 deletions(-)
 create mode 100644 pkg/gentoo/rvm-1.1.4.ebuild

  RVM:  Shell scripts enabling management of multiple ruby environments.
  RTFM: http://rvm.beginrescueend.com/
  HELP: http://webchat.freenode.net/?channels=rvm (#rvm on irc.freenode.net)

Upgrading the RVM installation in /Users/gdansk/.rvm/
    Correct permissions for base binaries in /Users/gdansk/.rvm/bin...
    Copying manpages into place.

Upgrade Notes

  * rvm_trust_rvmrcs has been changed to rvm_trust_rvmrcs_flag for consistency

  * Ruby package dependency list for your OS is given by:
    rvm notes

  * If you encounter any issues with a ruby 'X' your best bet is to:
    rvm remove X ; rvm install X

  * If you wish to have the 'pretty colors' again, set in ~/.rvmrc:
    export rvm_pretty_print_flag=1

  * If you see the following error message: Unknown alias name: 'default'
    re-set your default ruby, this is due to a change in how default works.

Upgrade of RVM in /Users/gdansk/.rvm/ is complete.

milligramme,

Thank you very much for using RVM! I sincerely hope that RVM helps to
make your work both easier and more enjoyable.

If you have any questions, issues and/or ideas for improvement please
join#rvm on irc.freenode.net and let me know, note you must register
(http://bit.ly/5mGjlm) and identify (/msg nickserv <nick> <pass>) to
talk, this prevents spambots from ruining our day.

My irc nickname is 'wayneeseguin' and I hang out in #rvm typically

  ~09:00-17:00EDT and again from ~21:00EDT-~23:00EDT

If I do not respond right away, please hang around after asking your
question, I will respond as soon as I am back.  It is best to talk in
#rvm itself as then other users can help out should I be offline.

Be sure to get head often as rvm development happens fast,
you can do this by running 'rvm get head' followed by 'rvm reload'
or opening a new shell

  w⦿‿⦿t

    ~ Wayne

Installed RVM HEAD version:

rvm 1.1.4 by Wayne E. Seguin (wayneeseguin@gmail.com) [http://rvm.beginrescueend.com/]
```

1.1.3から1.1.4にアップデートされました。

一度開きなおして新規シェルにします。

### 対象にしたいRuby（[Gemset]）を選択して wrapper コマンドを実行

```bash
$ rvm wrapper 1.9.2@rails2 textmate
```

`~/.rvm/bin/textmate_ruby` に wrapper script が生成される

TextMateのアップデートとBundle（バンドル）のアップデートを

シェルスクリプトで実行

（TextMateなら下のスクリプトをコピーして、Shell ScriptバンドルのRunで実行出来る！）

```bash
#!/usr/bin/env bash
mkdir -p /Library/Application\ Support/TextMate/
sudo chown -R $(whoami) /Library/Application\ Support/TextMate
cd /Library/Application\ Support/TextMate/
if [[ -d Bundles/.svn ]] ; then
  cd Bundles && svn up
else
  if [[ -d Bundles ]] ; then
    mv Bundles Bundles.old
  fi
  svn co http://svn.textmate.org/trunk/Bundles
fi
exit 0
```

### ログ

```bash
A    Bundles/Lisp.tmbundle
A    Bundles/Lisp.tmbundle/Commands
A    Bundles/Lisp.tmbundle/Commands/Help.tmCommand
A    Bundles/Lisp.tmbundle/Commands/Documentation for Word.tmCommand
A    Bundles/Lisp.tmbundle/Preferences
A    Bundles/Lisp.tmbundle/Preferences/Miscellaneous.tmPreferences
A    Bundles/Lisp.tmbundle/Preferences/Comments.tmPreferences
A    Bundles/Lisp.tmbundle/Macros
A    Bundles/Lisp.tmbundle/Macros/Overtype ')'.plist
A    Bundles/Lisp.tmbundle/Snippets
A    Bundles/Lisp.tmbundle/Snippets/'(.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/if.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/defparameter.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/defmacro.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/defun.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/setf.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/let.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/let1.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/`(.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/defvar.tmSnippet
A    Bundles/Lisp.tmbundle/Snippets/defconstant.tmSnippet
A    Bundles/Lisp.tmbundle/info.plist
A    Bundles/Lisp.tmbundle/Support
A    Bundles/Lisp.tmbundle/Support/help.markdown
A    Bundles/Lisp.tmbundle/Syntaxes
A    Bundles/Lisp.tmbundle/Syntaxes/Lisp.plist
A    Bundles/Ruby on Rails.tmbundle
A    Bundles/Ruby on Rails.tmbundle/LICENSE

#====================================================
#中略
#====================================================

A    Bundles/Transmit.tmbundle/Support/bin/upload_4.applescript
 U   Bundles
Checked out revision 11995.
```

TextMateの初期設定メニューの
Textmate | Preferences | Advanced | Shell Variables
で

Variable: TM_RUBY
Value: /Users/ユーザ名/.rvm/bin/textmate_ruby

と設定

### Bundlerをリネームすることで設定が有効にする。

```bash
$ cd /Applications/TextMate.app/Contents/SharedSupport/Support/lib/ ; mv Builder.rb Builder.rb.backup
```

### 1.9系のRubyのためのplistのアップデート。

```bash
$ git clone git://github.com/kballard/osx-plist.git
Cloning into osx-plist...
remote: Counting objects: 162, done.
remote: Compressing objects: 100% (74/74), done.
remote: Total 162 (delta 78), reused 162 (delta 78)
Receiving objects: 100% (162/162), 27.37 KiB, done.
Resolving deltas: 100% (78/78), done.

$ cd osx-plist/ext/plist/

$ ruby extconf.rb && make
creating Makefile
gcc -I. -I/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302/lib/ruby/1.8/i686-darwin10.5.0 -I/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302/lib/ruby/1.8/i686-darwin10.5.0 -I. -I/Users/gdansk/.rvm/usr/include  -D_XOPEN_SOURCE -D_DARWIN_C_SOURCE   -fno-common -g -O2 -pipe -fno-common   -c plist.c
cc -dynamic -bundle -undefined suppress -flat_namespace -o plist.bundle plist.o -L. -L/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302/lib -L/Users/gdansk/.rvm/usr/lib -L.  -framework CoreFoundation -undefined suppress -flat_namespace    -ldl -lobjc  

$ cp plist.bundle /Applications/TextMate.app/Contents/SharedSupport/Support/lib/osx/
```

これで完了、サンプルを用意します。

![/images/2010/12/ruby_rb.png](/images/2010/12/ruby_rb.png)

### TextMateのRuby（Gemset）切り替えはこんな感じで


```bash
$ rvm wrapper 1.8.7@rails2 textmate
```

![/images/2010/12/rvm_tm_187.png](/images/2010/12/rvm_tm_187.png)


```bash
$ rvm wrapper 1.9.2@rails2 textmate
```

![/images/2010/12/rvm_tm_192.png](/images/2010/12/rvm_tm_192.png)

ちゃんと切り替わってます。

rvm gemsetの切り替えだけではダメなのですが、これで使いやすくなった。