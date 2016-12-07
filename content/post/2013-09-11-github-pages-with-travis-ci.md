+++
title = "Travis CI で github pagesを更新"
date = "2013-09-11T00:00:00+09:00"
tags = ["githubpages", "middleman"]
+++

jekyll でやっていたこのブログを middleman-blog に変更したので、ついでにデプロイ自動化したかったのでそのメモ。

### やりたいこと
github にプッシュしたら [Travis CI](https://travis-ci.org/) で build して github の master ブランチにデプロイしてほしい


1. middleman-blog をつくる
1. `.nojekyll` を生成して jekyell を無効にしておく
1. github の 編集用ブランチ src に push
1. travis ci で clone して build
1. `./build` のなかみを github の master ブランチに push する

### やること

travis の gem をインストールしておく

```
$ gem install travis
$ travis init
```

`.travis.yml` が生成されるので自分の環境にあわせて編集

```markdown
language: ruby
rvm:
- 2.0.0

env:
  global:
    - GIT_COMMITTER_NAME=milligramme
    - GIT_COMMITTER_EMAIL=milligramme.cc@gmail.com
    - GIT_AUTHOR_NAME=milligramme
    - GIT_AUTHOR_EMAIL=milligramme.cc@gmail.com
```

Travis から Github に push するために GITHUB TOKEN `GH_TOKEN` を用意する

[Authorized Applications](https://github.com/settings/applications) で Create new token する。わかりやすい名前をつけて作成

Travis にログイン

```
$ travis login
We need your GitHub login to identify you.
This information will not be sent to Travis CI, only to api.github.com.
The password will not be displayed.

Try running with --github-token or --auto if you don't want to enter your password anyways.

Username: milligramme
Password: ******************

Successfully logged in!
```

`travis encrypt` コマンドで Travis の `SECURE_KEY` を取得する

```
$ travis encrypt -r milligramme/milligramme.github.io GH_TOKEN=<GH_TOKEN>
```

`secure:"<SECURE_KEY>"` が返ってくるのでそのまま `.travis.yml` に記載

```markdown
env:
  global:
    - secure: "<SECURE_KEY>"
```


`.travis.yml` src ブランチにプッシュしたときのみ build するように blacklist/whitelist を設定。

```markdown
branches:
  only:
    - src
  except:
    - master
```

Rakefile に記述したスクリプトを実行する。Rakefile はほぼ [こちらの Rakefile](https://github.com/hokaccha/webtech-walker/blob/master/Rakefile) を参照。

 `username.github.io` では gh-page ブランチではなく、master ブランチに展開するの
git clone するところを src ブランチ指定 -b オプション などの変更を加えている [Rakefile](https://github.com/milligramme/milligramme.github.io/blob/src/Rakefile)


```markdown
# .travis.yml
before_script:
  - '[ "$TRAVIS_BRANCH" == "src" ] && [ "$GH_TOKEN" ] && rake setup'

script: rake build

after_success:
  - '[ "$TRAVIS_BRANCH" == "src" ] && [ "$GH_TOKEN" ] && rake publish'
```

これで、src ブランチに push すると Rakefile の task を実行し、master ブランチに static な html がデプロイされる。


### 参考にした
- [ブログをJekyllからmiddlemanに移行してTravis CIでGitHub Pagesにデプロイするようにした - Webtech Walker](http://webtech-walker.com/archive/2013/08/jekyll_to_middleman.html)
- [Middleman で作った web サイトを Travis + GitHub pages でお手軽に運用する - tricknotesのぼうけんのしょ](http://tricknotes.hateblo.jp/entry/2013/06/17/020229)
- [Travis CI: Configuring your build](http://about.travis-ci.org/docs/user/build-configuration/)
