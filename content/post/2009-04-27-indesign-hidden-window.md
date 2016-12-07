+++
title = "窓の外でごにょごにょ"
date = "2009-04-27T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
小耳にはさんだ。

`activeDocument`

よりは

`documents[0]`

の方が確実？

最前面が対象になるのは変わらないけど、ドキュメントがハイド状態でも対応できるらしい。

ハイド状態って最小化状態とは違うのだろうか？

InDesignCS3で

```js
app.windows[0].minimize();
alert(app.documents.****);
```

と

```js

app.windows[0].minimize();
alert(app.activeDocument.****);
```

でいろいろと試してみても違いがまだわからない。

090528Thu 全然違うみたい。

```js

app.open(inddファイル,false);
```

とやってゴニョゴニョするみたい。

使うときが来たらのメモ。