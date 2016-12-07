+++
title = "obj.hasOwnProperty（prop）のかわりにprop in objをつかう"
date = "2012-07-23T00:00:00+09:00"
tags = ["extendscript"]
+++

とあるオブジェクトが、テキストオブジェクトかどうかとかそういう場面で、

今まで、 `obj.hasOwnProperty(prop)` をつかっていたのだけど、

`for (var prop in Obj) {}` の `prop in Obj` の部分を使うのがいいらしいので試してみた。

```js
var text_obj = app.selection[0];
text_obj.hasOwnProperty('baseline') ;
// => true
'baseline' in text_obj;
// => ture
```

どちらも同じ結果が返る、手にもやさしいので今度からこちらを使うつもり。
