+++
title = "座標の取得と移動"
date = "2009-04-06T00:00:00+09:00"
tags = ["extendscript", "indesign"]

outdated = true
+++

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2654](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2654)

- 全ページに1点ずつ画像が配置
- 原点センターから上下に1mm
- 原点ノド元から0.5mmプラスし
- 画像フレームの原点を小口から10mm、天から15mmに設定をしなければなりません。

だそうだ。

（メモ）オブジェクトのサイズを取得するには面倒くさがらないこと。

- 線幅込みの座標 〜.visbleBounds;
- 線幅抜きの座標　〜.geometricBounds;
- 座標の配列　[y1, x1, y2, x2]

```js
(function(){
  var docObj=app.activeDocument;
  var pageObj=docObj.pages;
  var pageWd=docObj.documentPreferences.pageWidth;
  for (var i=0; i < pageObj.length; i++){
    var pageItm = pageObj[i].pageItems;
    for( var j=0; j < pageItm.length; j++){
      var yx1yx2 = pageItm[j].geometricBounds;
      var y1 = yx1yx2[0];
      var x1 = yx1yx2[1];
      var y2 = yx1yx2[2];
      var x2 = yx1yx2[3];
      if (pageObj[i].side == PageSideOptions.rightHand){
        pageItm[0].geometricBounds = [y1-0.5,x1,y2+0.5,x2+0.5];
        x2 = x2+0.5;
        y1 = y1-0.5;
        pageItm[0].move("undifined",[pageWd-x2-10,15-y1]);
      }
      else{
        pageItm[0].geometricBounds = [y1-0.5,x1-0.5,y2+0.5,x2];
        pageItm[0].move(["10mm","15mm"]);
      }
    }
  }
})();
```

自分なりにやろうとすると、きっと、とりあえず動くというベタベタ状態になっている気がする。

スマートな思考を目指そう。