+++
title = "OSX Mavericksにアップグレード"
date = "2015-07-20T00:00:00+09:00"
tags = ["osx"]
+++

やっとのことで、OSXを 10.8 Snow Leopard から 10.9 Mavericks にアップグレードした。

### やったこと

- homebrew でもろもろ `brew unlink` して `brew reinstall`
- adobe csのためにjavaのインストール
- onyxのバージョン合わせ
- native extensionをつかったgemの再インストール

をとりあえず実行。

### 問題点

launchctlが nothing found to load を吐く

- [coderwall\.com : establishing geek cred since 1305712800](https://coderwall.com/p/nrbxpa/max-os-x-launchctl-load-nothing-found-to-load)
- [Solving launchctl load “nothing found to load” error \| DaveOnCode](http://www.daveoncode.com/2013/02/01/solve-mac-osx-launchctl-nothing-found-to-load-error/)

    launchctl load -w -F /path/to/myplist.plist
    
wとFオプションを一度つけたらいい。

あとは、いまのところ問題なさそう。

### 地味に改善されたこと

- MacBook Air + Adobe Illustrator CS5のカーソル劇薄問題が解消
- CP932のtxtの QuickLook が文字化けしなくなった

