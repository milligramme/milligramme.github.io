+++
title = "XML要素との関係解除"
date = "2009-10-15T00:00:00+09:00"
tags = ["indesign", "extendscript", "xml"]
+++
[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3677http://forums.adobe.com/thread/506764?tstart=0](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3677http://forums.adobe.com/thread/506764?tstart=0)

をみててXMLで制作って普段ないので自信ないのですが、
「流用してください」ともらった素材が使わないのにXMLタグまみれのとき、気持ちが悪いので、
Root以下のタグ消すのという一緒かな？

```js
// XML要素とレイアウト情報の関係解除？
var doc = app.documents[0];

//Root以下をすべて解除
var xmlElmnt = doc.xmlElements[0].xmlElements.everyItem();
xmlElmnt.untag();
```

#### 091020(tue)1300

InD-Boardのコメントにあるように変更、一気に解除。