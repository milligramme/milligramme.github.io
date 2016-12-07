+++
title = "RVM seppuku して再インストールメモ"
date = "2011-07-28T00:00:00+09:00"
tags = ["development", "ruby", "rvm"]

outdated = true
+++

rvm をアップデートしたら、いろいろ不具合があって、

$ rvm seppuku (rvm inplode) で一度 ~/.rvm をまっさらにして再インストールしたのでメモ。


```
gdansk@ ~♪ rvm seppuku
WARN: Are you SURE you wish for rvm to implode?      
This will recursively remove /Users/gdansk/.rvm and other rvm traces?      
(type 'yes' or 'no')>
```
seppuku...

```
gdansk@ ~♪ bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)
Cloning into rvm...
remote: Counting objects: 5250, done.
remote: Compressing objects: 100% (2491/2491), done.
remote: Total 5250 (delta 3408), reused 3752 (delta 2051)
Receiving objects: 100% (5250/5250), 1.67 MiB | 214 KiB/s, done.
Resolving deltas: 100% (3408/3408), done.

  RVM:  Shell scripts enabling management of multiple ruby environments.
  RTFM: https://rvm.beginrescueend.com/
  HELP: http://webchat.freenode.net/?channels=rvm (#rvm on irc.freenode.net)

Installing RVM to /Users/gdansk/.rvm/
    Correct permissions for base binaries in /Users/gdansk/.rvm/bin...
    Copying manpages into place.

  Notes for Darwin ( Mac OS X )
    For Lion, Rubies should be built using gcc rather than llvm-gcc. Since
    /usr/bin/gcc is now linked to /usr/bin/llvm-gcc-4.2, add the following to
    your shell's start-up file: export CC=gcc-4.2
    (The situation with LLVM and Ruby may improve. This is as of 07-23-2011.)

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

Installation of RVM to /Users/gdansk/.rvm/ is complete.

milligramme,

Thank you very much for using RVM! I sincerely hope that RVM helps to
make your life both easier and more enjoyable.

If you have any questions, issues and/or ideas for improvement please
join#rvm on irc.freenode.net and let me know, note you must register
(http://bit.ly/5mGjlm) and identify (/msg nickserv <nick> <pass>) to
talk, this prevents spambots from ruining our day.

My irc nickname is 'wayneeseguin' and I am typically in #rvm during EDT daytime

If I do not respond right away, please hang around after asking your
question, I will respond as soon as I am back.  It is best to talk in
#rvm itself as then other users can help out should I be offline. The community
is growing and there are a lot of great people in it who gladly help if they can.

Be sure to get head often as rvm development happens fast,
you can do this by running 'rvm get head' followed by 'rvm reload'
or opening a new shell

  w⦿‿⦿t

    ~ Wayne

gdansk@ ~♪ cd
gdansk@ ~♪ rvm reload
RVM reloaded!
```


rvm install 完了

package をインストール

rvn package から rvm pkg にコマンドが変わった（helpはなおってない）

```
gdansk@ ~♪ rvm pkg install readline
Fetching readline-5.2.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1989k  100 1989k    0     0   228k      0  0:00:08  0:00:08 --:--:--  423k
Extracting readline-5.2.tar.gz to /Users/gdansk/.rvm/src
Applying patch '/Users/gdansk/.rvm/patches/readline-5.2/shobj-conf.patch'...
Configuring readline in /Users/gdansk/.rvm/src/readline-5.2.
Compiling readline in /Users/gdansk/.rvm/src/readline-5.2.
Installing readline to /Users/gdansk/.rvm/usr
Fetching readline-6.0.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 2217k  100 2217k    0     0   205k      0  0:00:10  0:00:10 --:--:--  372k
Extracting readline-6.0.tar.gz to /Users/gdansk/.rvm/src
Configuring readline in /Users/gdansk/.rvm/src/readline-6.0.
Compiling readline in /Users/gdansk/.rvm/src/readline-6.0.
Installing readline to /Users/gdansk/.rvm/usr
gdansk@ ~♪ rvm pkg install openssl
Fetching openssl-0.9.8n.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3681k  100 3681k    0     0   122k      0  0:00:30  0:00:30 --:--:--  187k
Extracting openssl-0.9.8n.tar.gz to /Users/gdansk/.rvm/src
Configuring openssl in /Users/gdansk/.rvm/src/openssl-0.9.8n.
Compiling openssl in /Users/gdansk/.rvm/src/openssl-0.9.8n.
Installing openssl to /Users/gdansk/.rvm/usr
gdansk@ ~♪ rvm pkg install zlib
Fetching zlib-1.2.5.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  531k  100  531k    0     0   108k      0  0:00:04  0:00:04 --:--:--  141k
Extracting zlib-1.2.5.tar.gz to /Users/gdansk/.rvm/src
Configuring zlib in /Users/gdansk/.rvm/src/zlib-1.2.5.
Compiling zlib in /Users/gdansk/.rvm/src/zlib-1.2.5.
Installing zlib to /Users/gdansk/.rvm/usr
gdansk@ ~♪ rvm pkg install iconv
Fetching libiconv-1.13.1.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 4605k  100 4605k    0     0   506k      0  0:00:09  0:00:09 --:--:--  651k
Extracting libiconv-1.13.1.tar.gz to /Users/gdansk/.rvm/src
Configuring libiconv in /Users/gdansk/.rvm/src/libiconv-1.13.1.
Compiling libiconv in /Users/gdansk/.rvm/src/libiconv-1.13.1.
Installing libiconv to /Users/gdansk/.rvm/usr
```

オプション付きで ruby をインストール

```
gdansk@ ~♪ rvm install 1.9.2 -C --enable-share,--with-readline-dir=$HOME/.rvm/usr,--with-iconv-dir=$HOME/.rvm/usr,--with-openssl-dir=$HOME/.rvm/usr,--with-zlib-dir=$HOME/.rvm/usr
Installing Ruby from source to: /Users/gdansk/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...

ruby-1.9.2-p290 - #fetching 
ruby-1.9.2-p290 - #downloading ruby-1.9.2-p290, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 8604k  100 8604k    0     0  1423k      0  0:00:06  0:00:06 --:--:-- 1824k
ruby-1.9.2-p290 - #extracting ruby-1.9.2-p290 to /Users/gdansk/.rvm/src/ruby-1.9.2-p290
ruby-1.9.2-p290 - #extracted to /Users/gdansk/.rvm/src/ruby-1.9.2-p290
Fetching yaml-0.1.4.tar.gz to /Users/gdansk/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  460k  100  460k    0     0   195k      0  0:00:02  0:00:02 --:--:--  302k
Extracting yaml-0.1.4.tar.gz to /Users/gdansk/.rvm/src
Configuring yaml in /Users/gdansk/.rvm/src/yaml-0.1.4.
Compiling yaml in /Users/gdansk/.rvm/src/yaml-0.1.4.
Installing yaml to /Users/gdansk/.rvm/usr
ruby-1.9.2-p290 - #configuring 
ruby-1.9.2-p290 - #compiling 
ERROR: Error running 'make ', please read /Users/gdansk/.rvm/log/ruby-1.9.2-p290/make.log
ERROR: There has been an error while running make. Halting the installation.
```

エラー

http://stackoverflow.com/questions/5671074/rvm-does-not-install-ruby-1-9-2-on-snow-leopard-error-running-make

を参考に readline の homebrew のパスを指定してリトライ

```
gdansk@ ~♪ rvm install 1.9.2 -C --enable-share --with-readline-dir=/usr/local/Cellar/readline/6.2.1/ --with-iconv-dir=$HOME/.rvm/usr --with-openssl-dir=$HOME/.rvm/usr --with-zlib-dir=$HOME/.rvm/usr
Installing Ruby from source to: /Users/gdansk/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...

ruby-1.9.2-p290 - #fetching 
ruby-1.9.2-p290 - #extracted to /Users/gdansk/.rvm/src/ruby-1.9.2-p290 (already extracted)
Fetching yaml-0.1.4.tar.gz to /Users/gdansk/.rvm/archives
Extracting yaml-0.1.4.tar.gz to /Users/gdansk/.rvm/src
Configuring yaml in /Users/gdansk/.rvm/src/yaml-0.1.4.
Compiling yaml in /Users/gdansk/.rvm/src/yaml-0.1.4.
Installing yaml to /Users/gdansk/.rvm/usr
ruby-1.9.2-p290 - #configuring 
ruby-1.9.2-p290 - #compiling 
ruby-1.9.2-p290 - #installing 
Retrieving rubygems-1.8.6
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  244k  100  244k    0     0   307k      0 --:--:-- --:--:-- --:--:-- 1063k
Extracting rubygems-1.8.6 ...
Removing old Rubygems files...
Installing rubygems-1.8.6 for ruby-1.9.2-p290 ...
Installation of rubygems completed successfully.
ruby-1.9.2-p290 - adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
ruby-1.9.2-p290 - #importing default gemsets (/Users/gdansk/.rvm/gemsets/)
Install of ruby-1.9.2-p290 - #complete 
gdansk@ ~♪ rvm use 1.9.2 --default
Using /Users/gdansk/.rvm/gems/ruby-1.9.2-p290
```


一応 1.8.7 もインストール

```
gdansk@ ~♪ rvm install 1.8.7 -C --enable-share --with-readline-dir=/usr/local/Cellar/readline/6.2.1/ --with-iconv-dir=$HOME/.rvm/usr --with-openssl-dir=$HOME/.rvm/usr --with-zlib-dir=$HOME/.rvm/usr
Installing Ruby from source to: /Users/gdansk/.rvm/rubies/ruby-1.8.7-p352, this may take a while depending on your cpu(s)...

ruby-1.8.7-p352 - #fetching 
ruby-1.8.7-p352 - #downloading ruby-1.8.7-p352, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 4108k  100 4108k    0     0   100k      0  0:00:40  0:00:40 --:--:--  582k
ruby-1.8.7-p352 - #extracting ruby-1.8.7-p352 to /Users/gdansk/.rvm/src/ruby-1.8.7-p352
ruby-1.8.7-p352 - #extracted to /Users/gdansk/.rvm/src/ruby-1.8.7-p352
ruby-1.8.7-p352 - #configuring 
ruby-1.8.7-p352 - #compiling 
ruby-1.8.7-p352 - #installing 
Removing old Rubygems files...
Installing rubygems-1.8.6 for ruby-1.8.7-p352 ...
Installation of rubygems completed successfully.
ruby-1.8.7-p352 - adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
ruby-1.8.7-p352 - #importing default gemsets (/Users/gdansk/.rvm/gemsets/)
Install of ruby-1.8.7-p352 - #complete 
gdansk@ ~♪ rvm list

rvm rubies

   ruby-1.8.7-p352 [ x86_64 ]
=> ruby-1.9.2-p290 [ x86_64 ]

gdansk@ ~♪ gem install bundler
ERROR:  Loading command: install (LoadError)
    dlopen(/Users/gdansk/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/1.9.1/x86_64-darwin10.8.0/zlib.bundle, 9): Library not loaded: libz.1.2.5.dylib
  Referenced from: /Users/gdansk/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/1.9.1/x86_64-darwin10.8.0/zlib.bundle
  Reason: image not found - /Users/gdansk/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/1.9.1/x86_64-darwin10.8.0/zlib.bundle
ERROR:  While executing gem ... (NameError)
    uninitialized constant Gem::Commands::InstallCommand
```

gem install しようとするとエラー

http://d.hatena.ne.jp/naberon/20110604/rvm_zlib_for_mac

を参考に

export DYLD_FALLBACK_LIBRARY_PATH=$HOME/.rvm/usr/lib/

を追加

```
gdansk@ ~♪ gem install bundler
Fetching: bundler-1.0.15.gem (100%)
Successfully installed bundler-1.0.15
1 gem installed
Installing ri documentation for bundler-1.0.15...
Installing RDoc documentation for bundler-1.0.15...
```

なぜか

irb 1.9.2で日本語できない

んー

