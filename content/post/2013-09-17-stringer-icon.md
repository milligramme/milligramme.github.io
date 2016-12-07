+++
title = "Stringerのアイコンをつくった"
date = "2013-09-17T00:00:00+09:00"
tags = ["oss"]
+++

Rssリーダに [Stringer](https://github.com/swanson/stringer) をつかっている。

iPhone でもつかっているけど、 apple-touch-icon がなくてホームスクリーンに保存したとき寂しいので自分用にアイコンつくった。

![2013 09 17 Stringer Icon](/images/2013-09-17_stringer_icon.png)

てらっと光沢のある処理をしてくれる `rel="apple-touch-icon"` は好きでないので `rel="apple-touch-icon-precomposed"` のほうにしてみた。

```html
<link rel="apple-touch-icon-precomposed" href="/path/to/apple-touch-icon-precomposed.png" />
```

一応プルリクしたら、ちょっと修正してよ、ついでに Favicon もね、とコメントをいただき、修正後採用されました。

最初は [S]じゃなくて[St]だっただった [modify apple-touch-icon character, match favicon with apple-touch-icon · 0adedf6 · swanson/stringer](https://github.com/swanson/stringer/commit/0adedf656440e9c3c8246d7d745ff1e98af965e5)

<!-- モチーフは アンチソーシャル、結界、RSSフィードのひろがり そんな感じです。 -->

### 参考にした

[WebのスキルでiPad/iPhoneアプリ風のWebアプリ作成のまとめ｜本を買わずに解決するWeb制作の小技](http://ameblo.jp/linking/entry-10766619832.html)