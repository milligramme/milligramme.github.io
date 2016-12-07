+++
title = "Tableオブジェクトの挿入位置"
date = "2012-06-27T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignの表（Table）って少し変態的な存在で扱いに困る。

テキストストーリー内の Tableオブジェクトの位置を取得するのに "0x22"の箇所でアンカーされているので、これを findGrep() などで見つければいける。DOMの上から攻める場合。

逆に Tableオブジェクトから（DOMの下から攻めて）親の挿入点を取得するのどうするんだろうと思って TableObject.properties 眺めてたら

```
storyOffset:resolve("/document[@id=1]//story[@id=235]/insertion-point[0]")
```

ってあったので、InsertionPoint オブジェクト取得できるっぽい。

これで、一時的に表をどっかに退避してまたもどすのとか、表をアンカー付オブジェクトにぶち込むとかいろいろ出来そう。

ちなみに、

ふつうUnicode的に"0x22"は 「" (QUOTATION MARK) 」のなのですが、スクリプティング的には SpecialCharacters.DOUBLE_STRAIGHT_QUOTE (1396986737) という定数を使っている。ややこしい