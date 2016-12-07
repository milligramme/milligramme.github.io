+++
title = "IllustratorからD&D　まとめ"
date = "2009-06-29T00:00:00+09:00"
tags = ["illustrator", "indesign"]
+++
IllustratorからInDesignにドラッグアンドドロップした文字の問題について、

- [InDesign_オーバープリント白の罠（IllustratorからのD&amp;D）](http://www.milligramme.cc/wp/2009/06/indesignillustratordd.html)
- [InDesign_グループなのにグループじゃない（IllustratorからD&amp;D）](http://www.milligramme.cc/wp/2009/06/indesign_illustratordd.html)

自分の技量では手に負えないので、ひとまず、まとめ。

IllustratorからInDesignにドラッグアンドドロップすると

いろんな事がおきます。


1. 文字は、アウトライン化されたPolygonでもTextFrameでもない、取得できない何かになる。もしくは何にもならない。
1. 文字だったオブジェクトの個数を取得できない。
1. 文字はアウトライン化されていると思いきや、フォント情報が残り、環境ないフォントの場合にも置き換えができない。
1. 白にオーバープリントがかかっている場合、スクリプトでの解除ができない。
1. 改行していると段落分のオブジェクトになる。
1. 塗りと線はそれぞれ別のオブジェクトになる。
1. アピアランスを使用していると、さらにレイヤー分のオブジェクトになる。
1. アピアランスで文字にグラデーションで塗りを使用していると、グラデーションを分割して一文字毎に別のオブジェクトになる。

### ■たとえばIllustratorでアピアランスをつかったフチ文字...

![/images/2010/09/500-dd_ai2id_1appearance.png](/images/2010/09/500-dd_ai2id_1appearance.png)

### ■ InDesignにドラッグアンドドロップすると、無いはずのフォント情報が...

![/images/2010/09/501-dd_ai2id_2embed_fonts.png](/images/2010/09/501-dd_ai2id_2embed_fonts.png)

### ■グループ解除するとこんなにバラバラに、しかもスクリプトで制御できません。

![/images/2010/09/502-dd_ai2id_3split_element.png](/images/2010/09/502-dd_ai2id_3split_element.png)

### 結論

Illustrator から InDesign に、<b>文字</b>をドラッグアンドドロップすると勝手にアウトライン化してくれて、<b>超便利</b>とかいっていると痛い目にあいます。

もしくは、周りの人を痛い目にあわせています。

文字情報のないパスは、白のオーバプリントを気をつければ、わりと無害みたい。