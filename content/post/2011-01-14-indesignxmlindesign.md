+++
title = "スタイル属性付XMLをInDesignに読み込む"
date = "2011-01-14T00:00:00+09:00"
tags = ["indesign", "xml"]

outdated = true
+++
先日の勉強会で @osimajp さんがデモしていた、InDesignのスタイル情報を含んだXMLの流し込みを忘れないうちにということで試してみました。

### 段落スタイル文字スタイル

```xml 
<p aid:pstyle="段落スタイル名">
<p aid:cstyle="文字スタイル名">
```

InDesignのスタイル属性は「aid:」をつかって付けられますが、pとimageタグ以外につけても無視されるで、段落・文字スタイルは全てpにあてます。

で、その外側をh1とかdescriptionとかspecとかのタグで包んであげる（divタグみたく）とInDesignの構造ビューで見やすくなるし、そのタグをターゲットにスクリプトによる後処理もできるとのことです。

調べていたら途中でtableとcellにスタイル割り当てられる「aid5:」（InDesign CS3から実装された機能だから）というのもあったのでついでに試してみました。

表に関しては、セルの結合や幅の指定もできるみたいなのですが、

セルの扱いが、HTMLでいう 

> table > tr > td の tr 

のようなものがわからなくて？、表のＲ行×Ｃ列の合計がセル数になるなら後は佳きに計らえ的になってしまったのだが実際はどうなんだろう。

### 表関係の属性

こんな感じでした。

```xml
<table aid5:tstyle="表スタイル名">
<cell aid5:cstyle="セルスタイル名">
<table aid:trows="表の行数">
<table aid:tcols="表の列数">
<cell aid:theader="ヘッダ">
<cell aid:tfooter="フッタ">
<cell aid:crows="結合する行数">
<cell aid:ccols="結合する列数">
<cell aid:ccolwidth="セル幅"> ->  (ポイント指定にしかならない)
```

で↓こんなXMLを書いてみて、


```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root xmlns:aid5="http://ns.adobe.com/AdobeInDesign/5.0/" xmlns:aid="http://ns.adobe.com/AdobeInDesign/4.0/">
<body>
  <h1>
    <p aid:pstyle="h1">Lorem ipsum dolor</p>
  </h1>
  <desc>
    <p aid:pstyle="p">Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. <p aid:cstyle="bold">Duis aute irure dolor in reprehenderit</p> in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </desc>
  <spec>
    <table aid:table="table" aid:trows="3" aid:tcols="4" aid5:tablestyle="tablest1">
      <cell aid:table="cell" aid:theader="" aid:crows="1" aid:ccols="4">Header!!</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="80" aid5:cellstyle="cellst1">1</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="120" aid5:cellstyle="cellst1">2</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="40" aid5:cellstyle="cellst1">3</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="180" aid5:cellstyle="cellst1">4</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="80" aid5:cellstyle="cellst1">5</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="120" aid5:cellstyle="cellst1">6</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="40" aid5:cellstyle="cellst1">7</cell>
      <cell aid:table="cell" aid:crows="1" aid:ccols="1" aid:ccolwidth="180" aid5:cellstyle="cellst1">8</cell>
    </table>
  </spec>
</body>
</root>
```

### InDesignドキュメントにXMLを読み込む

（予めスタイルを割り当てておく）と、構造ビューはこんな感じに

![/images/2011/01/structure_view.png](/images/2011/01/structure_view.png)

### ドキュメント内にドラッグアンドドロップしてあげると…

![/images/2011/01/place_xml_into_doc.png](/images/2011/01/place_xml_into_doc.png)

手書きで書いたXMLなのでインデントとか余分な改行が残っていますが、ドラッグアンドドロップだけで、タグにスタイルの割り当てがされました。

次回はオブジェクトスタイル付の画像について挑戦してみるつもり。
