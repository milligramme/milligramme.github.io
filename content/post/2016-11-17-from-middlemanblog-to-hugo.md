+++
title = "middleman-blogからhugoに移行を検討"
date = "2016-11-17T09:23:00+09:00"
tags = ["hugo", "middleman", "gitlab"]

outdated = false
+++

middlemanの記事生成が遅さが気になりだして、middleman blogから [Hugo](https://gohugo.io/) blogへの移行を検討している。

homebrew でインストール

```
% brew install hugo
% hugo version
Hugo Static Site Generator v0.17 BuildDate: 2016-11-07T23:42:05+09:00
```

サイトの生成

```
% hugo new site hugoblog

% cp -R middleman_blog/post hugoblog/content
% cp -R middleman_blog/images hugoblog/static
```

で雛形生成してmiddlemanのデータをざっくりコピー

記事内のfrontmatterはそのまま `yaml` でもいけるけど `toml` 形式に変換し、
仮themeを選択して config.toml を作成。

```
% hugo -D --watch server
```

ローカルサーバを起動


```
% hugo
```

で `hugoblog/public` にサイトが生成される、速い。


ただし問題点があって、

middleman-blogで生成していた urlが

post/2013-05-28-jsx-appencoding.html.md

https://milligramme.github.io/2013/05/28/jsx-appencoding.html

```toml
canonifyurls = true
```

https://milligramme.github.io/2013-05-28-jsx-appencoding/

にしても

```toml
canonifyurls = false
```

https://milligramme.github.io/2013-05-28-jsx-appencoding.html

にしても変わってしまう。


別途試用していた [GitLab Pages \- GitLab Documentation](https://docs.gitlab.com/ee/pages/README.html) がそのままhugoのdeployに対応していて、wreckr + github-pagesでやってたことを gitlabだけでできるのはいい。

urlを保持しないで github.io から gitlab.io に移行する方向に傾いてる。


