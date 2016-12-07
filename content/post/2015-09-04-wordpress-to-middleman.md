+++
title = "WordPressブログをmiddlemanのGitHubPagesに統合"
date = "2015-09-04T00:00:00+09:00"
tags = ["wordpress", "githubpages", "middleman"]
+++

WordPressの古いブログ記事を middlemanベースのGithub Pagesに移行・統合した

### 変換に使ったもの

WordPressからエキスポートしておいた wordpress.xml を

[salmansqadeer/wordpress\-to\-middleman](https://github.com/salmansqadeer/wordpress-to-middleman)

使って markdownに変換

個人的に気になる部分を修正して、

[add some options · milligramme/wordpress\-to\-middleman@2d1a5d9](https://github.com/milligramme/wordpress-to-middleman/commit/2d1a5d9ef46bb5cbfde0b0ba96bd3c2f500844a9)

再度変換

- htmlタグはほとんどそのまま残るので markdown記法に変換
- 画像はそのままだと元の `wp-content/uploads` のパスを参照してるので middleman のimages フォルダにダウンロードしてきて、リンクを修正
- .htaccess に `http://www.milligramme.cc/wp/xxxx` が `http://milligramme.github.io/yyyy/mm/zzzz` にリダイレクトされるよう記述

### やってないこと

- サイト内リンク（途中で面倒になった）
- コメント（disqusをまた使おうかと一瞬思ったけどやめた）