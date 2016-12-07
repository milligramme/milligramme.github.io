+++
title = "IDML由来のinddかそうでないかを調べる"
date = "2016-08-22T13:20:00+09:00"
tags = ["indesign", "xmp", "extendscript"]

outdated = false 
+++

[bug in InDesign XXXStyle\.previewColor](https://gist.github.com/milligramme/416d6cb51a8cb555052b)

IDML由来のinddだとただでもずれてしまう `Style.previewColor` の色が「さらに」変わってしまうバグがあるので、区別したかった。

GUIだと

InDesignメニュー > InDesign について... を cmd 押しながら選ぶと出てくる、
ドキュメント情報で 「InDesign 変換システムから開く」 が「はい」か「いいえ」によって、
区別できると思われる。

```js
var t = app.findKeyStrings("InDesign 変換システムから開く");
$.writeln(t); // $ID/Opened from InDesign Interchange
```

英語だと、たぶん "Opened from InDesign Interchange" なので、
この辺のキーワードで探していたものの情報が見つからず、
なにげに、

ファイルメニュー ＞ ファイル情報... ＞ Rawデータをみていたら

idml.xml

```xml
<xmpMM:InstanceID>xmp.iid:e5f9f261-5276-4ac4-856a-8e1cac81caf5</xmpMM:InstanceID>
<xmpMM:OriginalDocumentID>xmp.did:1b1baf83-05dc-418e-9180-0533b5bac268</xmpMM:OriginalDocumentID>
<xmpMM:DocumentID>xmp.did:db7765ea-89a3-4cc0-9f23-070ac04fe5be</xmpMM:DocumentID>
```

indd.xml

```xml
<xmpMM:InstanceID>xmp.iid:36e98f2d-278c-4070-a620-55d9b85ee785</xmpMM:InstanceID>
<xmpMM:DocumentID>xmp.did:1b1baf83-05dc-418e-9180-0533b5bac268</xmpMM:DocumentID>
<xmpMM:OriginalDocumentID>xmp.did:1b1baf83-05dc-418e-9180-0533b5bac268</xmpMM:OriginalDocumentID>
```

`xmpMM:DocumentID` と `xmpMM:OriginalDocumentID` の出現順が違うっぽいので、これを元に書いてみた。

`DocumentID` と `OriginalDocumentID` の属性値が同一かどうかは、別名保存すると変わるので使えない。


XMPのRawデータ相当のxmlを取得するのに [Extract Metadata with Adobe XMP \[Part 1\] \| IndiSnip \[InDesign® Snippets\]](https://indisnip.wordpress.com/2010/08/13/extract-metadata-with-adobe-xmp/) を参考にした。

<script src="https://gist.github.com/milligramme/99373913b57f72f753d902c2391ad822.js"></script>


