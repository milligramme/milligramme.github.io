+++
title = "middleman-blogからHugoに移行を検討"
date = "2016-11-17T09:23:00+09:00"
tags = ["hugo", "middleman", "gitlab"]

outdated = false
+++

middlemanの記事生成が遅さが気になりだして、middleman blogから [Hugo](https://gohugo.io/) blogへの移行を検討している。

[Hugo \- Hugo Quickstart Guide](https://gohugo.io/overview/introduction)

### Hugoのインストール

goの環境はすでにできているので、hugo をhomebrewでインストール

```
% brew install hugo
% hugo version
Hugo Static Site Generator v0.17 BuildDate: 2016-11-07T23:42:05+09:00
```

### サイトの生成

雛形を生成してmiddlemanのデータをざっくりコピー

```
% hugo new site hugoblog

% cp -R middleman_blog/post hugoblog/content
% cp -R middleman_blog/images hugoblog/static
```

記事内のfrontmatterはそのまま `yaml` でもいけるけど `toml` 形式に変換し、
仮themeを選択して配置、config.toml を作成。

config.tomlのパラメータはテーマによって変わってくる


### ローカルサーバを起動

```
% hugo --buildDrafts --watch server
```

### 静的htmlを生成

サーバ起動で `--watch` オプションをつけていると生成されない

```
% hugo
```

で `hugoblog/public` にサイトが生成される、速い。


### URLがかわる

ただし問題点があって、

middleman-blogで生成していたurlがファイルのprefixの年月日で分割して

post/2013-05-28-jsx-appencoding.html.md

https://milligramme.github.io/2013/05/28/jsx-appencoding.html

になっているのですが、

hugoだと

```toml
canonifyurls = true
```

https://milligramme.github.io/post/2013-05-28-jsx-appencoding/

にしても

```toml
canonifyurls = false
```

https://milligramme.github.io/post/2013-05-28-jsx-appencoding.html

にしても変わってしまう。


別途試用していた [GitLab Pages \- GitLab Documentation](https://docs.gitlab.com/ee/pages/README.html) がそのままhugoのdeployに対応していて、wreckr + github-pagesでやってたことを gitlabだけでできるのはいい。

urlを保持しないで github.io から gitlab.io に移行する方向に傾いてる。


