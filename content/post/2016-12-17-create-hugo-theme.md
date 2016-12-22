+++
outdated = false
title = "Hugoテーマを作る"
description = ""
tags = [
"hugo", "golang"
]
date = "2016-12-17T10:07:05+09:00"
categories = [
]
draft = true

+++

## Hugoのテーマを作る

公式のドキュメントをみてつくってみた

- [Hugo \- Creating a Theme](https://gohugo.io/themes/creation/)
- [Hugo \- Customizing a Theme](https://gohugo.io/themes/usage)


基本方針は

- グリッドシステム
- シンタックスハイライト
- 検索機能
- アイコンフォント

### グリッドシステム

[Pure](http://purecss.io/) をつかう

5または24分割で指定するグリッドシステム、その他、ボタンなどが少々ある。


### シンタックスハイライト

[Hugo \- Syntax Highlighting](https://gohugo.io/extras/highlighting#client-side)

のうち highlight.js をつかう

[Getting highlight\.js](https://highlightjs.org/download/)

CDN経由だと、主要な22の言語にしか対応していないので、別途追加する。



### 検索機能

[Hugo に全文検索を取り付けた \| the right stuff](http://rs.luminousspice.com/hugo-site-search/)

を参考に、サイト全体のjsonを生成して、jsでインクリメントサーチする。


### アイコンフォント

[Font Awesome, the iconic font and CSS toolkit](http://fontawesome.io/)


## はまりどころ

### list.htmlだとエラーになる

`layouts/_default/list.html` がどうしてもエラーになってしまう

[Only layouts/\_default/list\.html will work for my post section \- support \- Hugo Discussion](https://discuss.gohugo.io/t/only-layouts--default-list-html-will-work-for-my-post-section/1491)

[Hugo \- Content Views](http://gohugo.io/templates/views/#li-html)

li.html にするべきらしい
