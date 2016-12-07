+++
title = "new MacBook Air setup Pt.3"
date = "2013-10-22T00:00:00+09:00"
tags = ["osx", "development", "defaults", "textmate"]
+++

増税前に新しい MacBook Air 11 を買ったのでセッティングメモ その3

ほぼ移行完了してきた

### Heroku Toolbelt をインストール
Heroku用のコマンドラインツールをpkgでインストール

[Heroku Toolbelt](https://toolbelt.herokuapp.com/)

```bash
$ heroku login
Enter your Heroku credentials.
Email: xxxxxx@xxxx.xxx
Password (typing will be hidden):*****
Authentication successful.
```

### TextMate2 のバンドルのインストール

```bash
$ cd ~/Library/Application\ Support/Avian/Bundles/
$ git clone git@github.com:milligramme/Jsx.tmbundle.git
$ git clone git@github.com:milligramme/basiljs.tmbundle.git
$ git clone git@github.com:milligramme/text.tmbundle.git
$ git clone git@github.com:milligramme/processing.tmbundle.git Processing.tmbundle
$ git clone git@github.com:milligramme/handcrafted-haml-textmate-bundle.git HAML-Handcrafted.tmbundle

$ cd ~/Library/Application\ Support/Avian/Pristine\ Copy/Bundles/
$ git clone https://github.com/ram-nadella/DashMate.tmbundle.git
$ git clone git://github.com/johnmuhl/html5.tmbundle.git HTML5.tmbundle
```

### defaults類

いままで [TinkerTool](http://www.bresink.com/osx/TinkerTool.html) 使ってたけどやめた

```bash
### textmate
# 選択状態でスコープをドキュメントのかわりに選択範囲をする
$ defaults write com.macromates.TextMate.preview findInSelectionByDefault -bool YES

### dock
# 隠れたアプリのアイコンを半透明
$ defaults write com.apple.dock showhidden -bool true

# 右下に配置
$ defaults write com.apple.dock orientation -string right
$ defaults write com.apple.dock pinning -string end

# 自動隠すときの遅延をなくす
$ defaults write com.apple.Dock autohide-delay -float 0


# スクリーンキャプチャー名
$ defaults write com.apple.screencapture name Screen\ Shot

# ウィンドウを開くアニメーション無効
$ defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false

# Library フォルダ表示
$ chflags nohidden ~/Library/
```

### Jason.app

JSON viewer。ディスコンになってたけど、代替になるアプリがみつからない

[Jason Download (Mac)](http://mac.softpedia.com/progDownload/Jason-Download-87407.html)

あとはおいおい気付いたら

### 参考にした

- [dotfiles/.osx at master · mathiasbynens/dotfiles](https://github.com/mathiasbynens/dotfiles/blob/master/.osx?os-x-10.8)
- [Hidden Settings · textmate/textmate Wiki](https://github.com/textmate/textmate/wiki/Hidden-Settings#controlling-font-smoothing)
- [dotfiles/.osx at master · mathiasbynens/dotfiles](https://github.com/mathiasbynens/dotfiles/blob/master/.osx?os-x-10.8)