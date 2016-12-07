+++
title = "underscore.jsをインクルードしたjsxはInDとAiでダブルクリック実行できない"
date = "2014-11-26T00:00:00+09:00"
tags = ["underscore.js", "extendscript"]
+++

underscore.jsをincludeしたjsx（#targetでアプリケーション指定してる）がある

```js
#target "InDesign"
#include "~/Documents/includes/underscore.js"

Boo = this.Boo || {};

Boo.mmmmm = function () {
  var emp = [];
  var ary = [1,2,3,4];
  var str = "1234"
  alert(_.isEmpty(emp));
  alert(_.contains(ary, "4"));
  alert(_.contains(ary, 4));
  alert(_.contains(str, "4"));
}
try {
  Boo.mmmmm();
}
catch(x_x){
  alert([
    x_x.message,
    x_x.line,
    x_x.stack,
    ].join("\n"));
}
finally {
  alert("Well?");
}
```

これをFinderでダブルクリックして

![2014 11 26 Extendscript Dialog](/images/2014-11-26-extendscript-dialog.png)

ESTK（ExtendScript Toolkit）から実行(「いいえ」を選択)はできるけど


アプリケーションから実行（「はい」を選択）させようとすると、エラー

![2014 11 26 Underscore Error](/images/2014-11-26-underscore-error.png)

`#include` をやめて、直接1ファイルに結合してもだめ

アプリケーションからのスクリプト実行

- InDesignでScriptPanelに入れた場合
- Illustratorで「ファイル＞スクリプト＞その他のスクリプト...」からの場合（#target Illustrator）

は実行できる

なぜか

Photoshop（#target Photoshop）だけ、ダブルクリック＋アプリケーションから実行できる

