+++
title = "縦中横にアミカケをつけるを少し助ける"
date = "2009-05-26T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

あかつきさんとこで [あかつき＠おばなのDTP稼業録 【InDesign】下線を使って文字にアミカケをつける](http://pocketdtp.blog16.fc2.com/blog-entry-248.html) 

<blockquote>
縦書きの場合、縦中横を設定した文字に対してはアミカケを行うことが出来ません。
</blockquote>

その昔InDesign CSのころやっていた、縦書きの縦中横の部分に下線のアミカケがかからないというのをむりやり解決してた方法をそのままJavaScriptで書いてみた。

### やっていること（スクリプトも手作業も一緒。）

■下線でアミカケをしても、縦中横のところにはかかりません。

![/images/2010/09/407-takegaki1.png](/images/2010/09/407-takegaki1.png)

■縦中横のあとに全角スペースいれる。（（注）全角スペースの縦中横解除）

![/images/2010/09/410-takegaki2.png](/images/2010/09/410-takegaki2.png)

■全角スペースのまえにカーニング「-1000」をいれる。

![/images/2010/09/413-takegaki3.png](/images/2010/09/413-takegaki3.png)

```js
/**
縦中横網かけ補助
"assist tatechuyoko line marking"
使い方：
縦書きで下線でアミカケ後、アミカケのかからない縦中横の文字のあと
にカーソルをいれて実行。段落設定の「自動縦中横設定...」、
文字設定の「縦中横」いずれで縦中横を設定した場合も対応。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
var docObj = app.activeDocument;
var selObj = docObj.selection[0];
var insPt = selObj.insertionPoints[0];
insPt.contents = "　";//全角スペースを挿入
selObj.kerningValue = -1000;//全角スペースの前にカーニングを「-1000」をいれる。
//挿入点の親さがし
var inStory = insPt.parentStory;
//挿入スペースの縦中横を解除
var embSp = inStory.characters.itemByRange(insPt.index, insPt.index);
embSp.tatechuyoko = false;
```