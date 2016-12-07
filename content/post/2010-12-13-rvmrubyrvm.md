+++
title = "Rubyバージョン管理システムRVM再インストールメモ"
date = "2010-12-13T00:00:00+09:00"
tags = ["gem", "rails", "ruby", "rvm"]

outdated = true
+++
Rubyのバージョン管理システムのRVMの挙動を理解するために一度アンインストールして再インストールしてみました。

### 一度rvm環境を消す。

> implode   - removes all ruby installations it manages, everything in ~/.rvm

```bash
$ rvm implode
```

本家を参考に
 [RVM: Ruby Version Manager - Installing RVM](http://rvm.beginrescueend.com/rvm/install/) 

### GitHub Repositoryからインストール

```bash
$ bash < <( curl http://rvm.beginrescueend.com/releases/rvm-install-head )
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
109   986  109   986    0     0   2721      0 --:--:-- --:--:-- --:--:--  7703
Cloning into rvm...
remote: Counting objects: 16213, done.
remote: Compressing objects: 100% (4141/4141), done.
remote: Total 16213 (delta 10935), reused 15861 (delta 10649)
Receiving objects: 100% (16213/16213), 2.90 MiB | 792 KiB/s, done.
Resolving deltas: 100% (10935/10935), done.

  RVM:  Shell scripts enabling management of multiple ruby environments.
  RTFM: http://rvm.beginrescueend.com/
  HELP: http://webchat.freenode.net/?channels=rvm (#rvm on irc.freenode.net)

Installing RVM to /Users/gdansk/.rvm/
    Correct permissions for base binaries in /Users/gdansk/.rvm/bin...
    Copying manpages into place.

  Notes for Darwin ( Mac OS X )
    For Snow Leopard be sure to have Xcode Tools Version 3.2.1 (1613) or later
    You should download the latest Xcode tools from developer.apple.com.
      (This is since the dvd install for Snow Leopard contained bugs).

    If you intend on installing MacRuby you must install LLVM first.
    If you intend on installing JRuby you must install the JDK.
    If you intend on installing IronRuby you must install Mono (version 2.6 or greater is recommended).

    To seamlessly abandon the Apple-installed system ruby (ruby 1.8.7 patchlevel 174 for Snow Leopard):

    rvm install 1.8.7 # installs patch 302: closest supported version
    rvm system ; rvm gemset export system.gems ; rvm 1.8.7 ; rvm gemset import system # migrate your gems
    rvm --default 1.8.7

  You must now complete the install by loading RVM in new shells.

  1) Place the folowing line at the end of your shell's loading files
     (.bashrc or .bash_profile for bash and .zshrc for zsh),
     after all PATH/variable settings:

     [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"  # This loads RVM into a shell session.

     You only need to add this line the first time you install rvm.

  2) Ensure that there is no 'return' from inside the ~/.bashrc file,
     otherwise rvm may be prevented from working properly.

  This means that if you see something like:

    '[ -z "$PS1" ] && return'

  then you change this line to:

  if [[ -n "$PS1" ]] ; then

    # ... original content that was below the '&& return' line ...

  fi # <= be sure to close the if at the end of the .bashrc.

  # This is a good place to source rvm v v v
  [[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"  # This loads RVM into a shell session.

EOF - This marks the end of the .bashrc file

     Be absolutely *sure* to REMOVE the '&& return'.

     If you wish to DRY up your config you can 'source ~/.bashrc' at the bottom of your .bash_profile.

     Placing all non-interactive (non login) items in the .bashrc,
     including the 'source' line above and any environment settings.

  3) CLOSE THIS SHELL and open a new one in order to use rvm.

Installation of RVM to /Users/gdansk/.rvm/ is complete.

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
```

.bash_profileへ設定の書き込み

前回の設定は生きてる

```bash
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"
```

新しいシェルを開いてチェック

”rvm is a function”となればok

```bash
$ type rvm | head -n1
rvm is a function

```

手動ロードできる

```bash
$ source ~/.rvm/scripts/rvm
```

依存関係を確認

```bash
$ rvm notes

  Notes for Darwin ( Mac OS X )
    For Snow Leopard be sure to have Xcode Tools Version 3.2.1 (1613) or later
    You should download the latest Xcode tools from developer.apple.com.
      (This is since the dvd install for Snow Leopard contained bugs).

    If you intend on installing MacRuby you must install LLVM first.
    If you intend on installing JRuby you must install the JDK.
    If you intend on installing IronRuby you must install Mono (version 2.6 or greater is recommended).

    To seamlessly abandon the Apple-installed system ruby (ruby 1.8.7 patchlevel 174 for Snow Leopard):

    rvm install 1.8.7 # installs patch 302: closest supported version
    rvm system ; rvm gemset export system.gems ; rvm 1.8.7 ; rvm gemset import system # migrate your gems
    rvm --default 1.8.7
```

Rubyインストールに何度も失敗

コンパイル中にエラーになる

```bash
$ rvm install 1.8.7
/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302, this may take a while depending on your cpu(s)...

ruby-1.8.7-p302 - #fetching 
ruby-1.8.7-p302 - #extracted to /Users/gdansk/.rvm/src/ruby-1.8.7-p302 (already extracted)
ruby-1.8.7-p302 - #configuring 
ruby-1.8.7-p302 - #compiling 
Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.8.7-p302/make.log
There has been an error while running make. Halting the installation.

$ rvm install 1.8.7 -C --enable-shared,--with-readline-dir=$HOME/.rvm/usr
/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302, this may take a while depending on your cpu(s)...

ruby-1.8.7-p302 - #fetching 
ruby-1.8.7-p302 - #downloading ruby-1.8.7-p302, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 4086k  100 4086k    0     0  1733k      0  0:00:02  0:00:02 --:--:-- 2173k
ruby-1.8.7-p302 - #extracting ruby-1.8.7-p302 to /Users/gdansk/.rvm/src/ruby-1.8.7-p302
ruby-1.8.7-p302 - #extracted to /Users/gdansk/.rvm/src/ruby-1.8.7-p302
ruby-1.8.7-p302 - #configuring 
ruby-1.8.7-p302 - #compiling 
Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.8.7-p302/make.log
There has been an error while running make. Halting the installation.

$ rvm install 1.8.6 --reconfigure --configure --with-readline-dir=$HOME/.rvm/usr
/Users/gdansk/.rvm/rubies/ruby-1.8.6-p399, this may take a while depending on your cpu(s)...

ruby-1.8.6-p399 - #fetching 
ruby-1.8.6-p399 - #downloading ruby-1.8.6-p399, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3879k  100 3879k    0     0  1746k      0  0:00:02  0:00:02 --:--:-- 2254k
ruby-1.8.6-p399 - #extracting ruby-1.8.6-p399 to /Users/gdansk/.rvm/src/ruby-1.8.6-p399
ruby-1.8.6-p399 - #extracted to /Users/gdansk/.rvm/src/ruby-1.8.6-p399
ruby-1.8.6-p399 - #configuring 
ruby-1.8.6-p399 - #compiling 
Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.8.6-p399/make.log
There has been an error while running make. Halting the installation.

$ rvm install 1.9.2-head
/Users/gdansk/.rvm/rubies/ruby-1.9.2-head, this may take a while depending on your cpu(s)...

ruby-1.9.2-head - #fetching 
Updating ruby from http://svn.ruby-lang.org/repos/ruby/branches/ruby_1_9_2
Copying from repo to src path...
Running autoconf
ruby-1.9.2-head - #configuring 
ruby-1.9.2-head - #compiling 
Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.9.2-head/make.log
There has been an error while running make. Halting the installation.

$ rvm install 1.9.2-head --reconfigure --configure --with-readline-dir=$HOME/.rvm/usr
/Users/gdansk/.rvm/rubies/ruby-1.9.2-head, this may take a while depending on your cpu(s)...

ruby-1.9.2-head - #fetching 
Downloading source from http://svn.ruby-lang.org/repos/ruby/branches/ruby_1_9_2.
Copying from repo to src path...
Running autoconf
ruby-1.9.2-head - #configuring 
ruby-1.9.2-head - #compiling 
Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.9.2-head/make.log
There has been an error while running make. Halting the installation.
```

こちらを参考にして
 [rvmを入れなおしたのでメモ - ser1zw::diary](http://d.hatena.ne.jp/ser1zw/20100804/1280933893) 

ライブラリーをインストールしてみる

```bash
$ rvm package install openssl
Fetching openssl-0.9.8n.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3681k  100 3681k    0     0    97k      0  0:00:37  0:00:37 --:--:--   99k
Extracting openssl-0.9.8n.tar.gz to /Users/gdansk/.rvm/src
Configuring openssl in /Users/gdansk/.rvm/src/openssl-0.9.8n.
Compiling openssl in /Users/gdansk/.rvm/src/openssl-0.9.8n.
Installing openssl to /Users/gdansk/.rvm/usr

$ rvm package install zlib
Fetching zlib-1.2.5.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  531k  100  531k    0     0    99k      0  0:00:05  0:00:05 --:--:--  127k
Extracting zlib-1.2.5.tar.gz to /Users/gdansk/.rvm/src
Configuring zlib in /Users/gdansk/.rvm/src/zlib-1.2.5.
Compiling zlib in /Users/gdansk/.rvm/src/zlib-1.2.5.
Installing zlib to /Users/gdansk/.rvm/usr

$ rvm package install readline
Fetching readline-5.2.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1989k  100 1989k    0     0   274k      0  0:00:07  0:00:07 --:--:--  448k
Extracting readline-5.2.tar.gz to /Users/gdansk/.rvm/src
Applying patch '/Users/gdansk/.rvm/patches/readline-5.2/shobj-conf.patch'...
Configuring readline in /Users/gdansk/.rvm/src/readline-5.2.
Compiling readline in /Users/gdansk/.rvm/src/readline-5.2.
Installing readline to /Users/gdansk/.rvm/usr
Fetching readline-6.0.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 2217k  100 2217k    0     0   309k      0  0:00:07  0:00:07 --:--:--  503k
Extracting readline-6.0.tar.gz to /Users/gdansk/.rvm/src
Configuring readline in /Users/gdansk/.rvm/src/readline-6.0.
Compiling readline in /Users/gdansk/.rvm/src/readline-6.0.
Installing readline to /Users/gdansk/.rvm/usr

$ rvm package install iconv
Fetching libiconv-1.13.1.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 4605k  100 4605k    0     0   638k      0  0:00:07  0:00:07 --:--:--  866k
Extracting libiconv-1.13.1.tar.gz to /Users/gdansk/.rvm/src
Configuring libiconv in /Users/gdansk/.rvm/src/libiconv-1.13.1.
Compiling libiconv in /Users/gdansk/.rvm/src/libiconv-1.13.1.
Installing libiconv to /Users/gdansk/.rvm/usr

$ rvm package install libxml2
Fetching libxml2-2.7.3.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 4677k  100 4677k    0     0  64303      0  0:01:14  0:01:14 --:--:--  113k
Extracting libxml2-2.7.3.tar.gz to /Users/gdansk/.rvm/src
Configuring libxml2 in /Users/gdansk/.rvm/src/libxml2-2.7.3.
Compiling libxml2 in /Users/gdansk/.rvm/src/libxml2-2.7.3.
Installing libxml2 to /Users/gdansk/.rvm/usr
```

インストールいけた

```bash
$ rvm install 1.8.7 -C "--enable-shared=true,--with-opt-dir=$HOME/.rvm/usr"
/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302, this may take a while depending on your cpu(s)...

ruby-1.8.7-p302 - #fetching 
ruby-1.8.7-p302 - #extracting ruby-1.8.7-p302 to /Users/gdansk/.rvm/src/ruby-1.8.7-p302
ruby-1.8.7-p302 - #extracted to /Users/gdansk/.rvm/src/ruby-1.8.7-p302
ruby-1.8.7-p302 - #configuring 
ruby-1.8.7-p302 - #compiling 
ruby-1.8.7-p302 - #installing 
ruby-1.8.7-p302 - #rubygems installing to ruby-1.8.7-p302
Retrieving rubygems-1.3.7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  284k  100  284k    0     0   193k      0  0:00:01  0:00:01 --:--:--  214k
Extracting rubygems-1.3.7 ...
ruby-1.8.7-p302 - adjusting #shebangs for (gem).
ruby-1.8.7-p302 - #importing default gemsets (/Users/gdansk/.rvm/gemsets/)
Install of ruby-1.8.7-p302 - #complete 

$ rvm install 1.9.2 -C "--enable-shared=true,--with-opt-dir=$HOME/.rvm/usr"
/Users/gdansk/.rvm/rubies/ruby-1.9.2-p0, this may take a while depending on your cpu(s)...

ruby-1.9.2-p0 - #fetching 
ruby-1.9.2-p0 - #extracting ruby-1.9.2-p0 to /Users/gdansk/.rvm/src/ruby-1.9.2-p0
ruby-1.9.2-p0 - #extracted to /Users/gdansk/.rvm/src/ruby-1.9.2-p0
ruby-1.9.2-p0 - #configuring 
ruby-1.9.2-p0 - #compiling 
ruby-1.9.2-p0 - #installing 
ruby-1.9.2-p0 - updating #rubygems for /Users/gdansk/.rvm/gems/ruby-1.9.2-p0@global
ruby-1.9.2-p0 - updating #rubygems for /Users/gdansk/.rvm/gems/ruby-1.9.2-p0
ruby-1.9.2-p0 - adjusting #shebangs for (gem).
ruby-1.9.2-p0 - #importing default gemsets (/Users/gdansk/.rvm/gemsets/)
Install of ruby-1.9.2-p0 - #complete 
```

### インストールされたRubyを確認

```bash
$ rvm list

rvm rubies

   ruby-1.8.7-p302 [ x86_64 ]
   ruby-1.9.2-p0 [ x86_64 ]
```

Ruby 1.8.7に切り替えて

```bash
$ rvm use 1.8.7
Using /Users/gdansk/.rvm/gems/ruby-1.8.7-p302

$ which ruby
/Users/gdansk/.rvm/rubies/ruby-1.8.7-p302/bin/ruby
```

それぞれのバージョンの Ruby に Rails用の gemset をつくる

@globalは共通でつかえるgemを入れておくところ？

gemsetを指定しないと @global？

```bash
$ rvm gemset list

gemsets for ruby-1.8.7-p302 (found in /Users/gdansk/.rvm/gems/ruby-1.8.7-p302)
global
```

gemset を Rails2用とRails3用につくる

```bash
$ rvm gemset create rails2
'rails2' gemset created (/Users/gdansk/.rvm/gems/ruby-1.8.7-p302@rails2).
$ rvm gemset create rails3
'rails3' gemset created (/Users/gdansk/.rvm/gems/ruby-1.8.7-p302@rails3).
```

gemset を切り替えるときは

```bash
$ rvm use 1.8.7@rails2
Using /Users/gdansk/.rvm/gems/ruby-1.8.7-p302 with gemset rails2
```

\--defaultをつける

次回以降デフォルトになる

```bash
$ rvm use 1.8.7@rails2 --default
```

gemset @rails2にRails2.3.10をインストール

```bash
$ gem install rails -v=2.3.10
Successfully installed activesupport-2.3.10
Successfully installed activerecord-2.3.10
Successfully installed rack-1.1.0
Successfully installed actionpack-2.3.10
Successfully installed actionmailer-2.3.10
Successfully installed activeresource-2.3.10
Successfully installed rails-2.3.10
7 gems installed
Installing ri documentation for activesupport-2.3.10...
Installing ri documentation for activerecord-2.3.10...
Installing ri documentation for rack-1.1.0...
Installing ri documentation for actionpack-2.3.10...
Installing ri documentation for actionmailer-2.3.10...
Installing ri documentation for activeresource-2.3.10...
Installing ri documentation for rails-2.3.10...
Installing RDoc documentation for activesupport-2.3.10...
Installing RDoc documentation for activerecord-2.3.10...
Installing RDoc documentation for rack-1.1.0...
Installing RDoc documentation for actionpack-2.3.10...
Installing RDoc documentation for actionmailer-2.3.10...
Installing RDoc documentation for activeresource-2.3.10...
Installing RDoc documentation for rails-2.3.10...
```

gemset  @rails3にRails3.0.3をインストール

```bash
$ gem install rails
Successfully installed activesupport-3.0.3
Successfully installed builder-2.1.2
Successfully installed i18n-0.5.0
Successfully installed activemodel-3.0.3
Successfully installed rack-1.2.1
Successfully installed rack-test-0.5.6
Successfully installed rack-mount-0.6.13
Successfully installed tzinfo-0.3.23
Successfully installed abstract-1.0.0
Successfully installed erubis-2.6.6
Successfully installed actionpack-3.0.3
Successfully installed arel-2.0.6
Successfully installed activerecord-3.0.3
Successfully installed activeresource-3.0.3
Successfully installed mime-types-1.16
Successfully installed polyglot-0.3.1
Successfully installed treetop-1.4.9
Successfully installed mail-2.2.12
Successfully installed actionmailer-3.0.3
Successfully installed thor-0.14.6
Successfully installed railties-3.0.3
Successfully installed bundler-1.0.7
Successfully installed rails-3.0.3
23 gems installed
Installing ri documentation for activesupport-3.0.3...
Installing ri documentation for builder-2.1.2...
ERROR:  While generating documentation for builder-2.1.2
... MESSAGE:   Unhandled special: Special: type=17, text="<!-- HI -->"
... RDOC args: --ri --op /Users/gdansk/.rvm/gems/ruby-1.8.7-p302@rails3/doc/builder-2.1.2/ri --title Builder -- Easy XML Building --main README --line-numbers --quiet lib CHANGES Rakefile README doc/releases/builder-1.2.4.rdoc doc/releases/builder-2.0.0.rdoc doc/releases/builder-2.1.1.rdoc --title builder-2.1.2 Documentation
(continuing with the rest of the installation)
Installing ri documentation for i18n-0.5.0...
Installing ri documentation for activemodel-3.0.3...
Installing ri documentation for rack-1.2.1...
Installing ri documentation for rack-test-0.5.6...
Installing ri documentation for rack-mount-0.6.13...
Installing ri documentation for tzinfo-0.3.23...
Installing ri documentation for abstract-1.0.0...
Installing ri documentation for erubis-2.6.6...
Installing ri documentation for actionpack-3.0.3...
Installing ri documentation for arel-2.0.6...
Installing ri documentation for activerecord-3.0.3...
Installing ri documentation for activeresource-3.0.3...
Installing ri documentation for mime-types-1.16...
Installing ri documentation for polyglot-0.3.1...
Installing ri documentation for treetop-1.4.9...
Installing ri documentation for mail-2.2.12...
Installing ri documentation for actionmailer-3.0.3...
Installing ri documentation for thor-0.14.6...
Installing ri documentation for railties-3.0.3...
Installing ri documentation for bundler-1.0.7...
Installing ri documentation for rails-3.0.3...
File not found: lib

```

builderのドキュメント作成中にエラー、最後にlibエラー？

Ruby 1.9.2に切り替えて同様に

```bash
$ rvm use 1.9.2@rails2
Using /Users/gdansk/.rvm/gems/ruby-1.9.2-p0 with gemset rails2

$ gem install rails -v=2.3.10
Successfully installed activesupport-2.3.10
Successfully installed activerecord-2.3.10
Successfully installed rack-1.1.0
Successfully installed actionpack-2.3.10
Successfully installed actionmailer-2.3.10
Successfully installed activeresource-2.3.10
Successfully installed rails-2.3.10
7 gems installed
Installing ri documentation for activesupport-2.3.10...
Installing ri documentation for activerecord-2.3.10...
Installing ri documentation for rack-1.1.0...
Installing ri documentation for actionpack-2.3.10...
Installing ri documentation for actionmailer-2.3.10...
Installing ri documentation for activeresource-2.3.10...
Installing ri documentation for rails-2.3.10...
Installing RDoc documentation for activesupport-2.3.10...
Installing RDoc documentation for activerecord-2.3.10...
Installing RDoc documentation for rack-1.1.0...
Installing RDoc documentation for actionpack-2.3.10...
Installing RDoc documentation for actionmailer-2.3.10...
Installing RDoc documentation for activeresource-2.3.10...
Installing RDoc documentation for rails-2.3.10...

$ rvm use 1.9.2@rails3
Using /Users/gdansk/.rvm/gems/ruby-1.9.2-p0 with gemset rails3

$ gem install rails
Successfully installed activesupport-3.0.3
Successfully installed builder-2.1.2
Successfully installed i18n-0.5.0
Successfully installed activemodel-3.0.3
Successfully installed rack-1.2.1
Successfully installed rack-test-0.5.6
Successfully installed rack-mount-0.6.13
Successfully installed tzinfo-0.3.23
Successfully installed abstract-1.0.0
Successfully installed erubis-2.6.6
Successfully installed actionpack-3.0.3
Successfully installed arel-2.0.6
Successfully installed activerecord-3.0.3
Successfully installed activeresource-3.0.3
Successfully installed mime-types-1.16
Successfully installed polyglot-0.3.1
Successfully installed treetop-1.4.9
Successfully installed mail-2.2.12
Successfully installed actionmailer-3.0.3
Successfully installed thor-0.14.6
Successfully installed railties-3.0.3
Successfully installed bundler-1.0.7
Successfully installed rails-3.0.3
23 gems installed
Installing ri documentation for activesupport-3.0.3...
Installing ri documentation for builder-2.1.2...
Installing ri documentation for i18n-0.5.0...
Installing ri documentation for activemodel-3.0.3...
Installing ri documentation for rack-1.2.1...
Installing ri documentation for rack-test-0.5.6...
Installing ri documentation for rack-mount-0.6.13...
Installing ri documentation for tzinfo-0.3.23...
Installing ri documentation for abstract-1.0.0...
Installing ri documentation for erubis-2.6.6...
Installing ri documentation for actionpack-3.0.3...
Installing ri documentation for arel-2.0.6...
Installing ri documentation for activerecord-3.0.3...
Installing ri documentation for activeresource-3.0.3...
Installing ri documentation for mime-types-1.16...
Installing ri documentation for polyglot-0.3.1...
Installing ri documentation for treetop-1.4.9...
Installing ri documentation for mail-2.2.12...
Installing ri documentation for actionmailer-3.0.3...
Installing ri documentation for thor-0.14.6...
Installing ri documentation for railties-3.0.3...
Installing ri documentation for bundler-1.0.7...
Installing ri documentation for rails-3.0.3...
Installing RDoc documentation for activesupport-3.0.3...
Installing RDoc documentation for builder-2.1.2...
Installing RDoc documentation for i18n-0.5.0...
Installing RDoc documentation for activemodel-3.0.3...
Installing RDoc documentation for rack-1.2.1...
Installing RDoc documentation for rack-test-0.5.6...
Installing RDoc documentation for rack-mount-0.6.13...
Installing RDoc documentation for tzinfo-0.3.23...
Installing RDoc documentation for abstract-1.0.0...
Installing RDoc documentation for erubis-2.6.6...
Installing RDoc documentation for actionpack-3.0.3...
Installing RDoc documentation for arel-2.0.6...
Installing RDoc documentation for activerecord-3.0.3...
Installing RDoc documentation for activeresource-3.0.3...
Installing RDoc documentation for mime-types-1.16...
Installing RDoc documentation for polyglot-0.3.1...
Installing RDoc documentation for treetop-1.4.9...
Installing RDoc documentation for mail-2.2.12...
Installing RDoc documentation for actionmailer-3.0.3...
Installing RDoc documentation for thor-0.14.6...
Installing RDoc documentation for railties-3.0.3...
Installing RDoc documentation for bundler-1.0.7...
Installing RDoc documentation for rails-3.0.3...
```

rvmはつかわない（システムのRubyを使う）

```bash
$ rvm reset

$ rvm use
Now using system ruby.

$ which ruby 
/usr/bin/ruby
```

再インストールしてみて、自分の中でまとまったイメージはこんな感じ

![rvm_village.jpg](/images/2010/12/rvm_village.jpg)

gems （クローゼット）の中に各種 gemset （Tシャツ）が収納、gem （柄）のプリントはそれぞれのgemset （Tシャツ）へして、global gemsetは下着みたいなもので重ね着感覚？
それぞれの ruby の出動要請に応じてクローゼットからTシャツ着替えて行く感じか。
切り替える環境が増えれば増えるほど.rvm が膨れていくのが想像出来る。