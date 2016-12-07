+++
title = "jsxの実行方法・アプリによるthisの違い、underscore.js含むjsxのダブルクリック起動できた"
date = "2014-11-27T00:00:00+09:00"
tags = ["extendscript"]
+++

jsxのダブルクリック起動でアプリ指定のが動作しなかった件、 @nbqx さんに指摘いただき、jsx内の `this` の違いによって underscore.js が動作しないことがわかった

```js
#include '/PATH/TO/underscore.js'
var _ = this._;
```

としてあげたら、動作するようになった


### InDesignとIllustratorは BridgeTalk

```js
#target "illustrator"
alert(this);
```

![2014 11 27 Illustrator Bridgetalk](/images/2014-11-27-illustrator-bridgetalk.png)

```js
#target "indesign"
alert(this);
```

![2014 11 27 Indesign Bridgetalk](/images/2014-11-27-indesign-bridgetalk.png)


### Photoshopは global
Photoshopだけ、挙動がちがったのは `this` が ExtendScriptなどからの起動とおなじく global だったから

```js
#target "photoshop"
alert(this);
```

![2014 11 27 Photoshop Global](/images/2014-11-27-photoshop-global.png)

