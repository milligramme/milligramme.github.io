+++
title = "条件分岐インクルードとバイナリjsxのインクルード"
date = "2010-12-10T00:00:00+09:00"
tags = ["extendscript"]
+++
支給スクリプト（例. supplied_script.js）に手を加えることなくパラサイトしたい。

普通、自分で書いたスクリプト（my_script.js）の中に

```js
#include "supplied_script.js"
```

を挿入したらいい。
my_script.jsを実行時にsupplied_script.jsも実行してくれる、関数ならそれを実行すればいい。

でも、同じような内容のスクリプトがバージョンごとに（sup4.js, sup5.js, sup6.js, sup7.js）なんてなっていると、この内容ならバージョン分岐してくれよという場合もあったりするわけですが（CS用は別格として）、支給品にそんなことをいっても仕方がないので、がんばってバージョン分岐させた自分のスクリプトの中でインクルードのバージョン分岐をしてみようと、何も考えずにswitch〜case 式、if 式などで

```js
switch(app.version.split('.')[0]){
  case "4":
  #include "sup4.js"
  ; break;
  case "5":
  #include "sup5.js"
  ; break;
  case "6": 
  #include "sup6.js"
  ; break;
  case "7":
  #include "sup7.js"
  ; break;
  default: ; break;
}
```

とやるとうまくいきません。すべてのインクルードを読みにいっちゃいます。

探してみたら、
 [■クリエイター手抜きプロジェクト［240］Adobe CS3/CS4編　Adobe JavaScriptのTips／古籏一浩 : 日刊デジタルクリエイターズ](http://blog.dgcr.com/mt/dgcr/archives/20100524140300.html) にそのまま載っていて

```js
eval( '#include ' + "スクリプトのパス" );
```

とするとうまくいきました。

```js
switch(app.version.split('.')[0]){
  case "4":
  eval ('#include '+"sup4.js");
  ; break;
  case "5":
  eval ('#include '+"sup5.js");
  ; break;
  case "6": 
  eval ('#include '+"sup6.js");
  ; break;
  case "7":
  eval ('#include '+"sup7.js");
  ; break;
  default: ; break;
}
```

実行中のスクリプトのパスは

```js
var file = new File(app.activeScript);
var fs = file.parent.fsName;
```

で得られる。fileにparentが使えるのは初めて知った。

ちなみに
バイナリ.jsx（.jsxbin）のインクルードはこれでもうまくいかない。

```js
// エラーになる
// #include 'b.jsxbin'

// エラーになる
eval('#include '+'b.jsxbin')

// これで行けた（フルパスでないとだめみたい）
var file = new File(app.activeScript);
var fs = file.parent.fsName;
$.evalFile( fs+"/"+'b.jsxbin' )

// あとは
app.doScript( ".jsxbinの改行を取って1行にしたソース")
```

とする必要があります。