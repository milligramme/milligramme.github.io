+++
title = "データ結合後テキストフレーム総連結"
date = "2009-03-27T00:00:00+09:00"
tags = ["indesign", "extendscript"]

outdated = true
+++



ここのBBSでの話題は練習課題によい。ただし、悩んでいると回答がすぐ出てしまうことがあります。

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2668](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2668)


自分ならばデータ結合を実行して、結合された全てのテキストフレームを連結。

最後の力技部分をスクリプトのお世話になる。

```js
var pages = app.activeDocument.pages;
for(var i=0; i < pages.length-1; i++){
  pages[i].textFrames[0].nextTextFrame = pages[i+1].textFrames[0];
}
```

条件

- データ結合したドキュメントが開いており直後で、
- 1ページに1テキストフレームがあり、
- それぞれが連結されていない状態での実行

を想定。もっとエラー処理が必要でしょう。