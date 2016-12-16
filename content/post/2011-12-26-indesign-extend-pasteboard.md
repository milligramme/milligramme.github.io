+++
title = "ドキュメントのペーストボードを拡張"
date = "2011-12-26T00:00:00+09:00"
tags = ["indesign", "extendscript"]

outdated = true
+++

![pastaboard.png](/images/2011/12/pastaboard.png)

InDesign の処理でペーストボード外のオブジェクトをどうしてもいじらなければならないとき、

そのまま無理やりペーストボード外のフレームに画像配置をやろうとするとエラーになる。


`pasteboardPreferences.pasteboardMargins` を使ってペーストボード領域をひろげる。

デフォルトは

`[-1, 72]`

([左右（-1はページ幅）, 上下]、単位はポイント)。

mm単位系になっていると

`[-0.35277777777778, 25.4] `
 
になるという素敵な仕様になっているので、単位をつけて設定してあげる。

```js
doc.pasteboardPreferences.pasteboardMargins = ['-1pt','288pt'];
```
