+++
date = "2016-12-27T09:54:33+09:00"
tags = ["textmate","ruby"]
description = ""
title = "Github-Markdown.tmbundleでCJKのヘッダを使えるようする"
outdated = false
draft = true
categories = []

+++

[MikeMcQuaid/GitHub\-Markdown\.tmbundle](https://github.com/MikeMcQuaid/GitHub-Markdown.tmbundle) でCJK文字のヘッダーが正常に表現できない件が、redcarpetのv3.4.0のアップデートで直った。

[Convert to HTML fail when header is only CJK characters · Issue \#16 · MikeMcQuaid/GitHub\-Markdown\.tmbundle](https://github.com/MikeMcQuaid/GitHub-Markdown.tmbundle/issues/16)


rubygemsが更新もされたので、redcarpet v3.4.0 が普通にインストールできるようになった。

TextMateのrubyは `system ruby` をつかっているので、そちらのgemをインストールする。

rbenvの system ruby/gem でインストールしようとすると弾かれるので、フルパスでインストール。

```
% /System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/ruby -v
ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin14]

% /System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/gem list redcarpet

*** LOCAL GEMS ***

redcarpet (3.3.4)

% sudo /System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/gem install redcarpet
Password:
Fetching: redcarpet-3.4.0.gem (100%)
Building native extensions.  This could take a while...
Successfully installed redcarpet-3.4.0
1 gem installed

% /System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/gem list redcarpet

*** LOCAL GEMS ***

redcarpet (3.4.0, 3.3.4)
```

TextMateを再起動したら、使えるようになっている。
