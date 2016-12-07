+++
title = "スウォッチの罠にはまる"
date = "2009-06-15T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
今考え中の進行の過程で、以前の誤りに気づいてしまいハマリ度が更にアップ。「 [InDesign_何色か知るべきか否か](http://www.milligramme.cc/wp/2009/05/indesign_5.html) 」のスクリプトでスウォッチの％濃度を設定して「新規スウォッチとして保存したカラー」が正しく表示されません

090616Tue
修正しました。

例えばC=50, M=0, Y=0, K=0のカラーを表現するのに、何通りかありますが、

### ■C=100を50％にするだけ。

![/images/2010/09/482-swatch_and_tint1.png](/images/2010/09/482-swatch_and_tint1.png)

### ■C=100を50%にして、「C=100の50%」というスウォッチを新規作成

数値がおかしい！！

![/images/2010/09/483-swatch_and_tint2.png](/images/2010/09/483-swatch_and_tint2.png)

### ■C=50のスウォッチ

![/images/2010/09/484-swatch_and_tint3.png](/images/2010/09/484-swatch_and_tint3.png)

■確認してみると。２番目のことを考慮してませんでした。

```js
var docObj=app.activeDocument;
var selObj=docObj.selection;
for(var j=0; j < selObj.length; j++){
  stCol=selObj[j].fillColor;
  stColTint=selObj[j].fillTint;
  stColName=selObj[j].fillColor.name;
  xi=selObj[j].geometricBounds[1];
alert("position x="+xi+"\r"+"color="+stCol.colorValue+"\r"+"tint="+stColTint+"\r"+"name="+stColName);
}
```

### ■左から順番に

![/images/2010/09/485-swatch_Info.png](/images/2010/09/485-swatch_Info.png)
