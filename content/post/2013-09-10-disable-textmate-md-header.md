+++
title = "TextMate2 でMarkdownのヘッダのサイズ変更を無効にする"
date = "2013-09-10T00:00:00+09:00"
tags = ["textmate", "markdown"]
+++

いつからか TextMate2 の Markdown で H1 > H6 の #ヘッダがぼよんぼよん大きくなるようになった。


### やりたいこと

今さらながら、この #スタイルのヘッダーが大きく表示されるのがイヤになってきたので無効にする方法を探した。

```markdown
# H1 header
## H2 header
### H3 header

H1 header
=========

H2 header
---------
```

![2013 09 10 Markdown](/images/2013-09-10_markdown.png)

### 方法
Markdown.tmbundle をみてもそれっぽいものがなく、調べてみたら Theme.tmbundle をいじればいいらしい。

Bundles > Edit Bundles... > Themes > Setting

* Markup: Heading 1
* Markup: Heading 2
* Markup: Heading 3
* Markup: Heading 4
* Markup: Heading 5
* Markup: Heading 6

の各設定

```plist
{  fontName = 'Baskerville';
  fontSize = '2.25em';
}
```

好みの設定に変更または削除して再起動すると反映される。

変更した差分のデルタバンドルが本体とは別に生成されるので、戻したくなったら `~/Library/Application Support/Avian/Bundles/Themes.tmbundle` を削除すればいい。


### 参考にした
- [osx - In Textmate2, how do you disable the header-styles in Markdown documents? - Stack Overflow](http://stackoverflow.com/questions/16258302/in-textmate2-how-do-you-disable-the-header-styles-in-markdown-documents)