+++
title = ".jsxbin（バイナリjsx）について"
date = "2010-11-11T00:00:00+09:00"
tags = ["illustrator", "indesign", "extendscript"]
+++
Extend Script Toolkit（ESTK）がいくらタコタコタコといっても、TextMateでは逆立ちしてもできないであろうことの一つが .jsxのバイナリ形式での書き出しです。

どうやら、この.jsxbinはCS3とCS4（たぶんCS5でも）の間で上位互換がある（下位互換がない？）ようなのでメモ。調べる発端は [こちら](http://ameblo.jp/knym71/entry-10703443761.html) から。

元のスクリプト

```js
alert("こんにちわーるど");
```

これを
ESTK 2（CS3）でバイナリ書き出し

```
@JSXBIN@ES@1.0@MAbyBn0ABJAnAEjzFjBjMjFjSjUBRBFeI2iThQ2kThQ2jLhQ2jBhQ2kPhQ2nchQ2k
LhQ2jJhQff0DzACByB
```

ESTK CS4でバイナリ書き出し

```
@JSXBIN@ES@2.0@MyBbyBn0ABJAnAEjzFjBjMjFjSjUBfRBFeI2iThQ2kThQ2jLhQ2jBhQ2kPhQ2nchQ
2kLhQ2jJhQff0DzACByB
```

バージョン番号らしき部分など微妙に違います。CS5もきっと違うと思います。（そろそろ入れよう）

気付いたこと

- CS4ではIllustratorもInDesignでもCS3のバイナリ.jsxを実行出来る
- Illustrator は.jsxbin 拡張子を認識できない？（グレーアウトする）ので .jsx とリネームする必要がある

![/images/2010/11/ai_select_dialog.png](/images/2010/11/ai_select_dialog.png)

- InDesignは.jsxbin 拡張子でも認識・実行できる

![/images/2010/11/id_scriptpanel.png](/images/2010/11/id_scriptpanel.png)

- ESTK CS4で書き出した .jsxbin（およびリネームした.jsx） はInDesign CS3でも Illustrator CS3でもエラーになる（エラーメッセージが@JSXBIN@ES@2.0ではじまってたら、CS3版を別途用意してもらえるとよい）

![/images/2010/11/id_alert_cs3.png](/images/2010/11/id_alert_cs3.png)

![/images/2010/11/ai_alert_cs3.png](/images/2010/11/ai_alert_cs3.png)

おまけ

InDesignの場合、doScript( ) の引数として**改行を除去した**ワンライン.jsxbinのコードを渡すという手をつかうとバージョン分岐できます。

（ネタ元：  [Indiscripts :: Binary JavaScript Embedment (CS4/CS5)](http://www.indiscripts.com/post/2010/04/binary-javascript-embedment-cs4-cs5) ）

```js
// alert("こんにちわーるど");
if (app.version.split('.')[0] == 5) {
  //cs3
  app.doScript("@JSXBIN@ES@1.0@MAbyBn0ABJAnAEjzFjBjMjFjSjUBRBFeI2iThQ2kThQ2jLhQ2jBhQ2kPhQ2nchQ2kLhQ2jJhQff0DzACByB");
};
if (app.version.split('.')[0] == 6) {
  //cs4
  app.doScript("@JSXBIN@ES@2.0@MyBbyBn0ABJAnAEjzFjBjMjFjSjUBfRBFeI2iThQ2kThQ2jLhQ2jBhQ2kPhQ2nchQ2kLhQ2jJhQff0DzACByB");
}
```

![/images/2010/11/helloworld.png](/images/2010/11/helloworld.png)
