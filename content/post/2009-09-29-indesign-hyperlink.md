+++
title = "hyperlinkの覚え書き"
date = "2009-09-29T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
そのうち我が身に降り掛かる予感がするので、

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3661](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3661) 

について予習しておこう。

脱線してハイパーリンクがテキストアンカーの時の扱いに悩んでいる間に、すでに 

[せうぞーさん](http://d.hatena.ne.jp/seuzo/20090929/1254181707) 

ところに正規表現を使った検索置換方法があがってました。


```js
var doc = app.documents[0];

// URLリンクをかえたい
var hyLinkURL = doc.hyperlinkURLDestinations;
var findU     = "http://www.hoge.net/123456";
var repU      = "http://www.hoge.net/page/123456/";

for(var u=0; u < hyLinkURL.length; u++){
  hyLinkURL[u].destinationURL.replace(findU,repU);
}
// テキストアンカーをかえたい？
var hyLinkTEXT = doc.hyperlinkTextDestinations;

for(var t=0; t < hyLinkTEXT.length; t++){
  hyLinkTEXT[t].destinationText; //insertionPointを返すけど使い道がわからない
}
// ドキュメント内のページをかえたい
var hyLinkPAGE = doc.hyperlinkPageDestinations;

for(var p=0; p < hyLinkPAGE.length; p++){
  hyLinkPAGE[p].destinationPage    = doc.pages[0];
  hyLinkPAGE[p].viewSetting        = HyperlinkDestinationPageSetting.FIT_HEIGHT;
  //  hyLinkPAGE[p].viewPercentage = 100; //5-4000;
}
// ハイパーリンク自体の設定をかえたい
var hyLink = doc.hyperlinks;

for(var j=0; j < hyLink.length; j++){
  with(hyLink[j]){
    name        = "hyperlink "+Math.random();
    visible     = true;
    borderColor = UIColors.CUTE_TEAL;
    borderStyle = HyperlinkAppearanceStyle.DASHED;
    highlight   = HyperlinkAppearanceHighlight.INVERT;
    width       = HyperlinkAppearanceWidth.THICK;
  }
}
```
