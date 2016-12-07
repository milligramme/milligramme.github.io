+++
title = "Bunderを使ったGem管理のメモ"
date = "2011-01-19T00:00:00+09:00"
tags = ["bundler", "ruby", "rails"]

outdated = true
+++

 [以前](http://www.milligramme.cc/wp/archives/3227) 、つくったRVM環境をGemsetsをつかわない環境に作り直したのでメモ。

Rubyのバージョン管理ツール  [RVM](http://rvm.beginrescueend.com/)  は 複数バージョンのRuby と 各RubyのGemをGemsets で管理をするつもりで環境を作っていたのですが、この間、Rails勉強会＠東京 に行ったときに、Gemの管理は** [Bundler](http://gembundler.com/) **で、RVM では Rubyのバージョン管理のみに使うのがいいんじゃないかとアドバイスを頂いて、いろいろ試してみました。
まず、Gemの管理というのがいまいちイメージがつかめていなかったのですがGemfileという設定ファイルで
<ul>
  <li>RailsアプリなどをGithubなどからダウンロード、必要なGemをBundlerでインストール</li>
  <li>Rails2アプリを作る、Gemfileを作成する、バージョンを固定</li>
  <li>Rails3アプリを作る、Gemfileは自動作成される、追記する、バージョンを固定</li>
  <li>空のディレクトリを作る、Gemfileを作成、Gemのインストール、Bundleコマンドで指定バージョンのRailsでアプリを作成する</li>
</ul>
というような使い方でいいのだろうか？

![/images/2011/01/bundler.png](/images/2011/01/bundler.png)

何はともあれ、Bundlerのインストール

```bash
$ gem install bundler

Fetching: bundler-1.0.8.gem (100%)
Successfully installed bundler-1.0.8
1 gem installed
Installing ri documentation for bundler-1.0.8...
Installing RDoc documentation for bundler-1.0.8...
```

Bundlerとドキュメントがインストールされました。
ドキュメントがいらなかったり、インストールに時間を掛けたくないなら

```bash
$ gem install bundler --no-ri --no-rdoc
```
とすればいい

### Gemfileの作成

Gemfileがないと bundle installしてもエラーになるだけなので、

Railsアプリ用のディレクトリを作ってそこに作成。

sourceを最低ひとつ指定する。
:rubygems は "http://rubygems.org" のシンボル（別名、エイリアスみたいな）

```ruby
source :rubygems
source "http://rubygems.org"

source :rubyforge
source "http://gems.rubyforge.org"

source :gemcutter
source "http://gemcutter.org"
```

Gemを最低ひとつ記載する
GemfileにGem名（とバージョン）を記入。

```ruby
source :rubygems

gem 'rails','2.3.10'
```

Gemfileのバージョン指定
Bundlerの [Gemfileのページ](http://gembundler.com/gemfile.html) の説明をみると演算子の使い方が

> Most of the version specifiers, like >= 1.0, are self-explanatory. The specifier ~> has a special meaning, best shown by example. ~> 2.0.3 is identical to >= 2.0.3 and < 2.1. ~> 2.1 is identical to >= 2.1 and < 2.2. ~> 2.2.beta will match prerelease versions like 2.2.beta.12. 

```ruby
gem "nokogiri"
gem "rails", "3.0.0.beta3"
gem "rack",  ">=1.0"
gem "thin",  "~>1.1"
```

nokogiriは最新版、railsは3.0.0.beta3のみ、rackは1.0以上、thinは1.1以上1.2未満のバージョンを指定ということになるわけだ。
他にも色々と指定ができるのだけどまたの機会に。

### bundler installを実行する

```bash
$ bundle install vendor/bundle

Fetching source index for http://rubygems.org/
Installing rake (0.8.7) 
Installing activesupport (2.3.10) 
Installing rack (1.1.0) 
Installing actionpack (2.3.10) 
Installing actionmailer (2.3.10) 
Installing activerecord (2.3.10) 
Installing activeresource (2.3.10) 
Installing rails (2.3.10) 
Using bundler (1.0.8) 
Your bundle is complete! It was installed into ./vendor/bundle
The path argument to `bundle install` is deprecated. It will be removed in version 1.1. Please use `bundle install --path vendor/bundle` instead.
```

を実行すると、依存関係にあるGemが vendor/bundle フォルダ内にインストールされ、Gemfile.lockというのができる。

bundle install とすると、現在使用中のRubyの $GEM_HOME にインストールされるので、（それがいいか悪いかわからないけど）インストール先を別に指定してみます。

「$ bundle install vendor/bundle」は、Bundler v1.1でもしかしたらなくなるかもしれないので、かわりに 「$ bundle install --path vendor/bundle」を使えと出たので次回はそうしたいと思います。

![/images/2011/01/bundle_install.png](/images/2011/01/bundle_install.png)

Gemfile.lockの中身は依存関係。この組み合わせなら大丈夫という動作保証みたいなものかな。

```ruby
GEM
  remote: http://rubygems.org/
  specs:
    actionmailer (2.3.10)
      actionpack (= 2.3.10)
    actionpack (2.3.10)
      activesupport (= 2.3.10)
      rack (~> 1.1.0)
    activerecord (2.3.10)
      activesupport (= 2.3.10)
    activeresource (2.3.10)
      activesupport (= 2.3.10)
    activesupport (2.3.10)
    rack (1.1.0)
    rails (2.3.10)
      actionmailer (= 2.3.10)
      actionpack (= 2.3.10)
      activerecord (= 2.3.10)
      activeresource (= 2.3.10)
      activesupport (= 2.3.10)
      rake (>= 0.8.3)
    rake (0.8.7)

PLATFORMS
  ruby

DEPENDENCIES
  rails (= 2.3.10)
```

GemのバージョンをGemfileの物に固定して

```bash
$ bundle lock
```

bundle execコマンドを使ってGemfileで指定のバージョンのRailsのアプリを作成する。

```bash
# rails 2の場合
$ bundle exec rails .

# rails 3の場合
$ bundle exec rails new .
```

![/images/2011/01/bundle_exec_rails.png](/images/2011/01/bundle_exec_rails.png)

なるほど、Bundlerは Rails のためってわけでなく、Sinatraとかでも使えるのですね。見たことがあると思いました（  [Lokkaのインストール](https://github.com/komagata/lokka/blob/master/README.ja.rdoc)  ）

### 参考にした
- [Bundler: The best way to manage Ruby applications](http://gembundler.com/) 
- [gem管理の新標準ツール&quot;Bundler&quot;のTips - 床のトルストイ、ゲイとするとのこと](http://d.hatena.ne.jp/mirakui/20100703/1278165723) 
- [bundlerのREADMEを読んでのメモ - おもしろWEBサービス開発日記](http://d.hatena.ne.jp/willnet/20100324/1269407621) 
- [HsbtDiary(2010-03-15)　■bundler の紹介](http://www.hsbt.org/diary/20100315.html) 