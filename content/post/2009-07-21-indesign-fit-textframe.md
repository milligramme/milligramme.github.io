+++
title = "入れ子テキストでフィット"
date = "2009-07-21T00:00:00+09:00"
tags = ["indesign"]

outdated = true
+++

InDesignCS3以降でテキストフレームのハンドルをダブルクリックすると「ぴったり」フィットする件 [InDesign：テキストボックスをぴったりさせる - DTP Transit](http://www.dtp-transit.jp/adobe/indesign/post_883.html) 

入れ子にしたテキストフレームでは、「ぴったり！」ができるのは下だけのようです。

![/images/2010/09/543-fit_base_content.png](/images/2010/09/543-fit_base_content.png)

InDesignCS3にてw90 h40のテキストフレーム（13Qのテキスト）を、w60 h50 線幅2mmの楕円内に入れ子にする。

![/images/2010/09/544-fit_original.png](/images/2010/09/544-fit_original.png)

「ぴったりクリック」をしようとしても、下中央のハンドルしか反応しません。

![/images/2010/09/553-fit_pittari.png](/images/2010/09/553-fit_pittari.png)

また、選択ツールで「内容を選択」してからでないとハンドルにアクセスできません。調子に乗って変なことするとだめってこと。ついでに、同様に「オブジェクトサイズの調整」での"Fit Options"の挙動を試してみると、引き続きw90 h40のテキストフレーム（13Qのテキスト）を、w60 h50 線幅2mmの楕円内に入れ子にする。

### 内容をフレームに合わせる

→文字サイズと水平比率が変わる。文字サイズ13Q x 楕円h（50-線幅2） ÷ テキストフレームh40　= 15.4Q（水平比率100% x （60-2） ÷ 90　= 64.4％になると思いきや、文字が大きくなった分？？）水平比率100% x 楕円w（60-2） ÷ テキストフレームw90　x ｛ 40 ÷ （50-2）｝= 53.7%

![/images/2010/09/549-fit2_content2frame.png](/images/2010/09/549-fit2_content2frame.png)

![/images/2010/09/545-fit_content2frame.png](/images/2010/09/545-fit_content2frame.png)

### フレームを内容に合わせる

→入れ子にしたテキストフレームの形状にかかわらず、親オブジェクトが四角形になる。

![/images/2010/09/550-fit2_frame2content.png](/images/2010/09/550-fit2_frame2content.png)

![/images/2010/09/546-fit_frame2content.png](/images/2010/09/546-fit_frame2content.png)

### 内容を縦横比率に応じて合わせる

→文字サイズが変わる文字サイズ13Q x 楕円w（60-2） ÷ テキストフレームw90　= 8.3777Q

![/images/2010/09/551-fit2_proportionally.png](/images/2010/09/551-fit2_proportionally.png)

![/images/2010/09/547-fit_proportionally.png](/images/2010/09/547-fit_proportionally.png)

### フレームに均等に流し込む

→文字サイズが変わる文字サイズ13Q x 楕円h（50-2） ÷ テキストフレームh40　= 15.4Q

![/images/2010/09/552-fit2_fill_proportionally.png](/images/2010/09/552-fit2_fill_proportionally.png)

![/images/2010/09/548-fit_fill_proportionally.png](/images/2010/09/548-fit_fill_proportionally.png)

VisibleBoundsではなくGeometricBoundsにfitしている。

後々なにかに使うかもしれないメモ。