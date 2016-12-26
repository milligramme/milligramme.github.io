+++
date = "2016-12-26T09:43:47+09:00"
title = "specific_installでgithubリポジトリ指定してインストール"
description = ""
tags = [
]
categories = ["gem"
]
draft = false
outdated = false

+++

TextMateのtmbundleでCJK文字のヘッダーが正常に表現できない件

[Convert to HTML fail when header is only CJK characters · Issue \#16 · MikeMcQuaid/GitHub\-Markdown\.tmbundle](https://github.com/MikeMcQuaid/GitHub-Markdown.tmbundle/issues/16)


上流の redcarpet が修正したっぽいので


[with\_toc\_data with Japanese · Issue \#538 · vmg/redcarpet](https://github.com/vmg/redcarpet/issues/538#issuecomment-259325915)

アップデートしようとしたら、まだ最新版に更新されてないようで `gem update redcarpet` しても v3.4.0 が入らない。

bundler だと `github:` オプションで指定ができるが、`gem install` でnpmのように githubのリポジトリを指定してインストールする方法がないかしらべた。


[rdp/specific\_install: rubygems plugin to allow you to install an "edge" gem straight from its github repository](https://github.com/rdp/specific_install)

を使うと github上の最新をインストールできる。

```
% gem list redcarpet

*** LOCAL GEMS ***

redcarpet (3.3.4)

% gem update redcarpet
Updating installed gems
Nothing to update

% gem install specific_install
Fetching: backports-3.6.8.gem (100%)
Successfully installed backports-3.6.8
Fetching: specific_install-0.3.2.gem (100%)
Successfully installed specific_install-0.3.2
2 gems installed

% gem specific_install -l https://github.com/vmg/redcarpet.git
/usr/local/bin/git
git installing from https://github.com/vmg/redcarpet.git
Cloning into '/var/folders/sc/krlryv1x2rx5ghj77f96dcyh0000gn/T/d20161226-17916-wer3fs'...
remote: Counting objects: 3399, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 3399 (delta 4), reused 0 (delta 0), pack-reused 3384
Receiving objects: 100% (3399/3399), 1.01 MiB | 587.00 KiB/s, done.
Resolving deltas: 100% (1924/1924), done.
WARNING:  pessimistic dependency on test-unit (~> 3.1.3, development) may be overly strict
  if test-unit is semantically versioned, use:
    add_development_dependency 'test-unit', '~> 3.1', '>= 3.1.3'
WARNING:  See http://guides.rubygems.org/specification-reference/ for help
  Successfully built RubyGem
  Name: redcarpet
  Version: 3.4.0
  File: redcarpet-3.4.0.gem
Building native extensions.  This could take a while...
Successfully installed

% gem list redcarpet

*** LOCAL GEMS ***

redcarpet (3.4.0, 3.3.4)
```

