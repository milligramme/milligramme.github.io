+++
title = "データ結合ソースをExcelで作るときの@問題"
date = "2009-09-04T00:00:00+09:00"
tags = ["xls", "indesign"]
+++

>  adobeindesign.ruから
>「 [Работа с Data Merge в Adobe InDesign CS2](http://adobeindesign.ru/2009/09/03/rabota-s-data-merge-v-adobe-indesign-cs2/#) 」〜「Adobe InDesignCS2のデータ結合でお仕事」（意訳）

InDesignのデータ結合ソースは、普段FileMaker Proで作ることが多いので、Excelはあまり使いません

Excelのデータ結合の画像用ヘッダとして「@」を（デフォルトのセル書式設定：標準では）1文字目に打てないというのの解決策。

他アプリから生成されたExcel書類などセル属性をいじれない場合などにつかえるのでメモ。

既出？（先日のDTP Booster 005の [YUJI](http://study-room.info/id/) さんのデモでも一度テキストを開いて「＠」をつけてたような...）

データ結合のソースをExcelで作るとき、通常画像を表すヘッダ「＠ふにふに」は、入力することができません、「その関数は無効です。」とかなんとか怒られてしまいます。

（セルの書式設定を「文字列」にすれば大丈夫）

＠の直前に **シングルクォーテーションマーク** 「'」を入力すると、とりあえず怒られません。※わかりにくいけど、Cのセルにはシングルクォーテーションマーク「'」が入ってます。

![/images/2010/09/610-excel_at_image.png](/images/2010/09/610-excel_at_image.png)

このExcelから、タブ区切りテキストを書出し、InDesign側でデータ結合のソースとして読み込みます。

すると、シングルクォーテーションマークをつけた＠のヘッダは、そのまま画像のヘッダとして認識してくれます。

ダブルクォーテーションマークだとダメなようです。

![/images/2010/09/611-data_merge_panel.png](/images/2010/09/611-data_merge_panel.png)
しっかり、画像としてプレースできます。

![/images/2010/09/612-data_merge_tag.png](/images/2010/09/612-data_merge_tag.png)

プレビューにするとちゃんと。

![/images/2010/09/613-data_merge_preview.png](/images/2010/09/613-data_merge_preview.png)

ちょっとだけ他の文字を試しましたが、「半角スペース」も大丈夫みたい。

### 20090904{fri)2000頃追記
  
RRRさんの指摘でちょっと追記

セルの書式設定を「文字列」にすれば大丈夫。