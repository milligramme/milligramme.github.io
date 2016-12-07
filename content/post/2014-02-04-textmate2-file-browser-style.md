+++
title = "TextMate2 ファイルブラウザの見た目変更"
date = "2014-02-04T00:00:00+09:00"
tags = ["extendscript", "defaults"]

outdated = true
+++

ファイルブラウザのスタイルを osx の [source list](https://developer.apple.com/library/mac/documentation/userexperience/conceptual/applehiguidelines/Windows/Windows.html#//apple_ref/doc/uid/20000961-CHDDIGDE) 形式に変更する

![Filebrowser Default Style](/images/2014-02-04_tm2_filebrowser_default_style.png)

```
$ defaults write com.macromates.TextMate.preview fileBrowserStyle SourceList
```

![Filebrowser Sourcelist Style](/images/2014-02-04_tm2_filebrowser_sourcelist_style.png)

戻す

```
$ defaults delete com.macromates.TextMate.preview fileBrowserStyle
```
