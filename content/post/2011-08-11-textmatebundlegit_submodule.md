+++
title = "TM Bundleをgit submodule管理に変更"
date = "2011-08-11T00:00:00+09:00"
tags = ["git", "textmate"]

outdated = true
+++
普段コーディングに使っているTextMateのバンドル（.tmbundle）とプラグイン（.tmplugin）の管理を git submodule に変えたのでメモ。

### 今まで

- インストールはshellscriptを用意して一気に  `git clone`。
- アップデートはそれぞれのバンドルのディレクトリで `git pull`する。
  
という方法を取っていましたが、

[@satococoa](http://twitter.com/#!/satococoa)  さんに git submodule 便利。
 
と教えてもらったので整頓も兼ねて試してみました。

### 基本的な流れ

- gitレポジトリ内（Bundlesフォルダ）で、`git submodule add [追加したいバンドルの gitリポジトリ]` で追加
- `git commit -m "add to submodule"` でコミット。これでOK。
- 更新する場合は `git submodule foreach git pull` という感じでアップデートがあれば更新してくれる。

やってることは `git pull` なんで、bundleをちょこっと自分仕様にいじったときでも、上書きでなくマージになるので安心（なはず）。

いっぱいある.tmbundleを各々 `git submodule add` が面倒なので、インストール用のshellscriptを流用してsubmodule化します。

```bash
#!/usr/bin/env bash

### Bundles
# cd /Users/`whoami`/Library/Application\ Support/TextMate/Bundles

# ExtendScript Bundle original
# git://github.com/kanemu/extendscript-tmbundle.git
git submodule add git@github.com:milligramme/extendscript-tmbundle.git ExtendScript.tmbundle

# Jsdoc-Toolkit Bundle
git submodule add http://github.com/choan/jsdoctoolkit-tmbundle.git Jsdoctoolkit.tmbundle

# AppleScript Bundle
git submodule add git://github.com/textmate/applescript.tmbundle.git AppleScript.tmbundle

# HTML5
git submodule add git://github.com/johnmuhl/html5.tmbundle.git

# kHTML xhtml
git submodule add git://github.com/kennethreitz/kHTML.tmbundle.git

# Ruby on Rails 3
git submodule add git://github.com/drnic/ruby-on-rails-tmbundle.git "Ruby on Rails.tmbundle"

# Titanium Mobile Bundle
git submodule add git://github.com/subtleGradient/JavaScript-Appcelerator-Titanium-Mobile.tmbundle.git TitaniumMobile.tmbundle

# TextMate Bundle for CoffeeScript
git submodule add git://github.com/jashkenas/coffee-script-tmbundle CoffeeScript.tmbundle

# Handcrafted HAML TextMate bundle
git submodule add git://github.com/handcrafted/handcrafted-haml-textmate-bundle.git HAML-Handcrafted.tmbundle

# SeaofClouds Sass TextMate bundle
git submodule add git://github.com/seaofclouds/sass-textmate-bundle.git "Ruby Saas.tmbundle"

# RSpec
git submodule add git://github.com/rspec/rspec-tmbundle.git RSpec.tmbundle
# open up TextMate Preferences
# go to the Advanced tab
# add a variable named RUBYOPT with the value rubygems

# RDoc
git submodule add git://github.com/joshaven/RDoc.tmbundle.git

# Markdown, Multi Markdown
git submodule add git://github.com/textmate/markdown.tmbundle.git
```

### <del datetime="2011-08-27T00:41:40+00:00">同様にPluginの方</del>

こちらはTextMateで日本語入力する上で必須の CJK-Input.tmplugin は github上にはないので別途インストール。

面倒なのでついでに shellscriptで落としてこれるように。

<del datetime="2011-08-27T00:41:40+00:00">また、git管理の対象外にしたいので .gitignore にディレクトリを追加しておきます。</del>


```bash
#!/usr/bin/env bash

# install CJK-Input.tmplugin

cd $HOME/Desktop
curl -O http://hetima.com/textmate/CJKInput20061110.zip
unzip CJKInput20061110.zip
cp -r CJKInput20061110/CJK-Input.tmplugin /Users/`whoami`/Library/Application\ Support/TextMate/PlugIns/CJK-Input.tmplugin
rm -rf CJKInput20061110.zip CJKInput20061110
rm -rf __MACOSX
```

2011-08-22
プラグインはsubmoduleではプラグインが動作しないのでやらないことにした

```bash
#!/usr/bin/env bash

### Plugin
# cd /Users/`whoami`/Library/Application\ Support/TextMate/PlugIns 

# MissingDrawer Plugin for TextMate
git submodule add git://github.com/jezdez/textmate-missingdrawer.git MissingDrawer.tmplugin
```


### 更新

自分の場合は平気でしたが、branchがmasterになってないバンドルもあるみたいなので、更新はこんな感じがいいみたい。

foreach以降は gitコマンドなんですね。

```bash
$ git submodule foreach 'git checkout master; git pull origin master'
```

長いな。エイリアスつくろう。

### 参考にした

- [textmateのbundleをgitで管理 - うんたらかんたら日記](http://d.hatena.ne.jp/rochefort/20110206/p1)
- [git submoduleの更新 - うんたらかんたら日記](http://d.hatena.ne.jp/rochefort/20110410/p1)
- [2.4.1 git-submoduleで外部のリポジトリを扱う。 - redtower&apos;s memo](http://redtower.plala.jp/2010/11/05/git-submodule.html)
