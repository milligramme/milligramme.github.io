+++
title = "PDF用テキストアンカーを自動で"
date = "2009-04-29T00:00:00+09:00"
tags = ["indesign", "pdf", "extendscript"]

outdated = true
+++
InDesignでリンク付きのインタラクティブなPDFをつくるのための下処理。オブジェクトにまとめて、後でつくる予定のPDFのハイパーリンク用にテキストアンカーを付けたいとき。とりあえずは表組の一つ目のセルの文字をアンカー名として使う方向でやってみる。あとで応用。■こんな感じに。

![/images/2010/09/368-hyperlink_anchor1.png](/images/2010/09/368-hyperlink_anchor1.png)

■小組がいっぱいとかそんなとき、実行後

![/images/2010/09/371-hyperlink_anchor2.png](/images/2010/09/371-hyperlink_anchor2.png)

■一つ目のセルの文字の前に...

![/images/2010/09/374-hyperlink_anchor3.png](/images/2010/09/374-hyperlink_anchor3.png)

■目次をつくり、ハイパーリンクを設定

![/images/2010/09/377-hyperlink_anchor4.png](/images/2010/09/377-hyperlink_anchor4.png)

テキストアンカーができてます。連番にしたらきれいに並びます。

```js
/*
PDFブックマーク用テキストアンカーを作成
"creat named interactive anchors for PDF bookmarks"
使い方：
実行するとデータ結合で作成された表組を含むテキストフレームの一つ目のセルに
そのセル内容の名前がついたテキストアンカーをつくります。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
var docObj = app.documents[0];
var pageObj = docObj.pages;
for (var i=0; i < pageObj.length; i++){
  var tfObj=pageObj[i].textFrames;
  for (var k=0; k < tfObj.length; k++){
    var tableObj=tfObj[k].tables;
    for (var j=0; j < tableObj.length; j++){
      //ハイパーリンク先にする名前
      var cellText=tableObj[j].cells[0].contents;
      var txtRange=tableObj[j].cells[0].texts[0];
      //名前をつけてテキストアンカーにする
      var lstLink=docObj.hyperlinkTextDestinations.add(txtRange,{name:cellText});
    }
  }
}
```