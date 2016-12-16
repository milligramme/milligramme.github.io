+++
outdated = false
title = "Hugoテーマを作る"
description = ""
tags = [
"hugo", "golang"
]
date = "2016-12-16T10:07:05+09:00"
categories = [
]
draft = true

+++

## Hugoのテーマを作る

公式のドキュメントをみて大体わかる

- [Hugo \- Creating a Theme](https://gohugo.io/themes/creation/)
- [Hugo \- Customizing a Theme](https://gohugo.io/themes/usage)


基本方針は

- レスポンシブデザイン
- シンタックスハイライト
- 検索機能
- フォント

### レスポンシブデザイン

[Pure](http://purecss.io/) をつかう

5または24分割で指定するグリッドシステム


### シンタックスハイライト

[Hugo \- Syntax Highlighting](https://gohugo.io/extras/highlighting#client-side)

のうち highlight.js をつかう

[Getting highlight\.js](https://highlightjs.org/download/)

CDN経由だと、主要な22の言語にしか対応していないので、別途追加する。



### 検索機能

[Hugo に全文検索を取り付けた \| the right stuff](http://rs.luminousspice.com/hugo-site-search/)

を参考に、サイト全体のjsonを生成して、インクリメントサーチ


### フォント

Google Fontsと Font Awesome


## はまりどころ

### list.htmlがエラーになる

`layoutes/_default/list.html` がどうしてもエラーになってしまう

```
Started building sites ...
ERROR: 2016/12/16 10:39:43 general.go:212: Error while rendering section : template: theme/_default/list.html:14:14: executing "theme/_default/list.html" at <.IsDraft>: can't evaluate field IsDraft in type *hugolib.Node
ERROR: 2016/12/16 10:39:43 general.go:212: Error while rendering section post: template: theme/_default/list.html:14:14: executing "theme/_default/list.html" at <.IsDraft>: can't evaluate field IsDraft in type *hugolib.Node
```

[Only layouts/\_default/list\.html will work for my post section \- support \- Hugo Discussion](https://discuss.gohugo.io/t/only-layouts--default-list-html-will-work-for-my-post-section/1491)

[Hugo \- Content Views](http://gohugo.io/templates/views/)

li.html にするべき
