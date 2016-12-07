+++
title = "middleman-blogをweckerでデプロイ"
date = "2016-01-19T00:00:00+09:00"
tags = ["middleman", "wecker", "deploy"]
+++

middleman-deploy もいいが、buildに結構時間がとられるので、
 [Wercker \- automation driven development](http://wercker.com/) を使うようにした。

travis-ciのときのように
srcブランチにpushしたら、masterにデプロイしてくれるようになる。

wrecker.yml を作成（softtab: 4 でないと動作しない）

```yml
box: wercker/rvm

build:
    steps:
        - rvm-use:
            version: 2.3.0

        - bundle-install:
            jobs: 4

        - script:
            name: build with middleman
            code: bundle exec middleman build --verbose

deploy:
    steps:
        - lukevivier/gh-pages:
            token: $GITHUB_TOKEN
            basedir: build
            repo: milligramme/milligramme.github.io
```

### 参考にした

- [wercker \- docs \- Deploy](http://devcenter.wercker.com/docs/deploy/index.html)
- [無料で werckerで RspecのCI & Capistrano Deploy \- 酒と泪とRubyとRailsと](http://morizyun.github.io/blog/wercker-ci-rspec-capistrano-deploy-auto/)
- [既存Railsアプリにwerckerを導入するまで \| スペースマーケットブログ](http://blog.spacemarket.com/code/install-worcker-ci/)
- [Middleman 製の GitHub Pages サイトを Wercker で自動デプロイ \- Qiita](http://qiita.com/shuhei/items/c295f061cba7b5f0d112)
