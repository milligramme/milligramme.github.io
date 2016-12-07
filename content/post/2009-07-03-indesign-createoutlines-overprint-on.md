+++
title = "グラフィック化でオーバープリントONになる"
date = "2009-07-03T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

今回は[紙色]は大丈夫。でもやっぱり危ない。

InDesign で通常、[黒]は特別な色として扱われていて

[CS2、CS3の場合] 環境設定＞黒の表示方法＞ [黒]スウォッチを100%でオーバープリントにチェック が入っていたら、個々にオーバープリントにチェックを入れなくても、[黒]100%はオーバープリントになります。

![/images/2010/09/513-black_is_overprint.png](/images/2010/09/513-black_is_overprint.png)

その設定で、[黒]テキストをグラフィック化すると、オーバープリントにチェックがかかる。

そのまま、知らずに後で色を変えたりすると...（以下略）

InDesign には、2種類のグラフィック化があります。

### ■テキストフレームを選択して実行**（下線、打ち消し線、段落境界線、親のテキストフレームなどが消える）

![/images/2010/09/514-blact_text.png](/images/2010/09/514-blact_text.png)

### ●オーバープリントにチェックが入る**（色をかえてもチェックママ）

![/images/2010/09/515-black_textframe_outline.png](/images/2010/09/515-black_textframe_outline.png)

### ●さすがに[紙色]に変更した場合はチェックが外れます

![/images/2010/09/517-outlined_text_set_paper.png](/images/2010/09/517-outlined_text_set_paper.png)

### ■テキストフレーム内のテキストを範囲選択して実行**（下線、打ち消し線は消える。段落境界線は残る。インラインオブジェクトになる）

![/images/2010/09/516-black_textrange_outline.png](/images/2010/09/516-black_textrange_outline.png)

やはり、オバプリオンでいっしょでした。（CS2とCS3の場合）

```js
var selObj=app.selection[0];
var outTex=selObj.createOutlines();
try{
  outTex[0].overprintFill=false;
}
catch(e){}

try{
  outTex[0].overprintStroke=false;
}
catch(e){}

//selObj.createOutlines(false);
//DeleteOriginalオプションを"false"にすると
//元のテキストを削除せずに、別オブジェクトとしてグラフィック化
```

090711Sat0000ころ。追記

さらに細かく検証されてるところによると、CS〜CS4でもなるのですね。

オーバープリントオフにするスクリプトを「グラフィック化」のショートカットにあてるか、こうなるものとして、人力チェックが必須なんてどうかしてる。 [[4008][InDesign]InDesignで[黒]の文字をグラフィックス化(アウトライン化)するとオーバープリントの属性が設定される](http://blog.ddc.co.jp/mt/dtp/archives/20090710/182300.html) 