+++
title = "CS3でボタンを小さくするにはminimumSizeをつかえばいい"
date = "2011-05-20T00:00:00+09:00"
tags = ["scriptui"]
+++
以前、Adobe CS3のスクリプトUIではボタン幅に制限があるというエントリー（ [[ScriptUI]CS3ではボタン幅を69px以下にできない | diary NET. 1.2mg](http://www.milligramme.cc/wp/archives/3088#respond) ）をかきましたが、
PICTRIXさんにコメントで解決策を教えて頂いたので早速試してみました。ありがとうございます。

```js

var w = new Window('dialog',"minimuSize");
w.add('statictext', undefined, "ScriptUI ver. " + $.version);
w.add('statictext', undefined, app.name + " " + app.version);
for (var j=0; j < 13; j++) {
  var g = w.add('group');
  g.spacing = 0;
  g.margins = 0;
  for (var i=0; i < 13; i++) {
    var b = g.add('button', undefined, ".");
    b.minimumSize = [18+j, 18+i];
    b.size = b.minimumSize;
    b.text =  (18+j) + "x" + (18+i);
  };
};
w.show();
```
InDesign CS3 - CS5 / OSX 10.6.7でチェック。
ボタン部分は、ずれてません。
![/images/2011/05/minimusize_compare.jpg](/images/2011/05/minimusize_compare.jpg)
