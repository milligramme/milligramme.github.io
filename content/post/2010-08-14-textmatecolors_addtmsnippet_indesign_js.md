+++
title = "colors_add.tmSnippet （InDesign JS用）"
date = "2010-08-14T00:00:00+09:00"
tags = ["textmate"]
+++
InDesignのExtendScript (JavaScript)などは、ふだんTextMateで書いているのですが、スニペット機能が便利で、思いついた（＝面倒くさいと思った）ときにちょろちょろ作ってたりします。

せっかく [ExtendScript TextMate Bundle](http://kanemu1117nc.blogspot.com/2010/06/extendscript-textmate-bundle.html)  を公開してくれたので援護射撃気味に.tmSnippetをアップしていこうと思います。

とりあえずはInDesignの[Color Object]の作成編

```js
docObj.colors[TAB]
```

↑このあとタブ押しで↓のように展開、「Process/Spot/Registration/MixedInkmodel」の部分を選択状態にして待機してくれます。

入力後またタブ押しで次のプロパティの値へと順に選択していきます。便利さが伝わるかな？

```js
docObj.colors.add({
model:ColorModel.Process/Spot/Registration/MixedInkmodel,
space:ColorSpace.CMYK/RGB/LAB/MixdInk,
colorValue:[0,0,0,0],
name:""});
```

colors_add.tmSnippet

<script src="http://gist.github.com/576964.js?file=colors_add.tmSnippet"></script>

ダウンロード後、ダブルクリックでインストール（中身はこんな感じ↑）