+++
title = "new MacBook Air setup Pt.5"
date = "2013-11-01T00:00:00+09:00"
tags = ["osx", "defaults"]
+++

今日の defaults メモ

![2013 11 01 Disable Warning](/images/2013-11-01-disable-warning.png)

### osxのfinderでファイルの拡張子を変更変更するとき警告を出さないようにする

```
$ defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false
```

### Dashboard無効

```
$ defaults write com.apple.dashboard mcx-disabled -boolean true
```

### スクリーンショットの影をつけない

```
$ defaults write com.apple.screencapture disable-shadow -bool true
```

### ウィンドウ上部にファイルパスを表示

```
$ defaults write com.apple.finder _FXShowPosixPathInTitle -bool true 
```

### open/saveダイアログで最近の項目数を増やす

```
$ defaults write -g NSNavRecentPlacesLimit -int 16
```