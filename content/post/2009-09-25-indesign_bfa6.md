+++
title = "InDesignでつぶつぶしたものをつなげる"
date = "2009-09-25T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
InDesignで円を円周に接しながら、大きさをランダムに増殖させてみた。

パラメータを調整＆固定すれば、ある程度方向性がでてきたり、直線状になるので、使い道があるかも。

いやどうだか。実行するとドキュメントの真ん中へんからずらずらとできます。設定によってはペーストボード外まで、はみ出しますが、最後にグループ化しますので、救出可能のはずです。

![img](/images/2010/09/639-beads_connection.jpg)

応用可能か如何に。

```js
/**
つぶつぶしたものをつなげる
"create thing like beads"
使い方：
最前面のドキュメントの中心付近から、透明度のちがうまるいものをずらずらとつなげます。
設定によりペーストボードをはみ出すことがあります。
オブジェクトはグループ化されます。
開いているドキュメントがなければ新規ドキュメントを勝手に作って実行します。
動作確認：OS10.4.11 InDesign CS3
milligramme
www.milligramme.cc
*/
if(app.documents.length == 0){
var doc=app.documents.add();
}
var doc=app.documents[0];
var dWidth=doc.documentPreferences.pageWidth;
var dHeight=doc.documentPreferences.pageHeight;
//ルーラーを一時的にスプレッドにする
var rulerBk=doc.viewPreferences.rulerOrigin;
var rulerTemp=RulerOrigin.SPREAD_ORIGIN;
doc.viewPreferences.rulerOrigin=rulerTemp;
//パラメータたち
var tubeValue=120;//つぶつぶのかず
var randMinR=2;//つぶつぶの最小半径
var randMaxR=5;//つぶつぶの最大半径
var randAngle=360;//ランダム角度のレンジ（0〜360）
var startR=randMinR+(randMaxR-randMinR)*Math.random();
var seedOval=doc.ovals.add();
seedOval.geometricBounds=[dHeight/2, dWidth/2, dHeight/2+2*startR, dWidth/2+2*startR];
seedOval.strokeColor="None";
seedOval.strokeWeight=0;
seedOval.fillColor="Black";
var seedCenter=[dWidth/2+startR, dHeight/2+startR]//x,y　最初のつぶのまんなか
//オブジェクトと半径と中心座標を配列にしていく
var familyG=new Array();
var familyR=new Array();
var familyC=new Array();
familyG.push(seedOval);
familyR.push(startR);
familyC.push(seedCenter);
//角度と半径をかえながら増殖
for(var i=0; i < tubeValue; i++){
var childR=randMinR+(randMaxR-randMinR)*Math.random();//あたらしい半径
var childRadi=randAngle*2*Math.PI/360*Math.random();//移動角度
var childCenter=[
familyC[i][0]+(familyR[i]+childR)*Math.sin(childRadi),
familyC[i][1]+(familyR[i]+childR)*Math.cos(childRadi)
];
var childOval=familyG[i].duplicate();
childOval.geometricBounds=[
familyC[i][1]+(familyR[i]+childR)*Math.cos(childRadi)-childR,
familyC[i][0]+(familyR[i]+childR)*Math.sin(childRadi)-childR,
familyC[i][1]+(familyR[i]+childR)*Math.cos(childRadi)+childR,
familyC[i][0]+(familyR[i]+childR)*Math.sin(childRadi)+childR
];
//不透明度を20％以上60％未満でランダムに
childOval.transparencySettings.blendingSettings.opacity=20+40*Math.random();
//配列に追加
familyR.push(childR);
familyG.push(childOval);
familyC.push(childCenter);
}
//オブジェクトの配列でグループ化
var familyP=doc.groups.add(familyG);
//ルーラーを元に戻す
doc.viewPreferences.rulerOrigin=rulerBk;
```