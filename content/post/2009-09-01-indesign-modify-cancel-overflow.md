+++
title = "オーバーフロー解除スクリプトたちを修正"
date = "2009-09-01T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
最近、以前に書いたスクリプトの修正が多い。

今回は表組のオーバーフロー解除スクリプトです。

1. [「InDesign選択したセルだけオーバーフロー解除」](/2009/04/23/indesign-cancel-cell-overflows.html)
1. [「InDesign_七歩進んで一歩戻る」](/2009/06/12/indesign-cancel-overflow.html)


これらのスクリプトで「連結テキストフレーム内に表組が連続していて、それがページをまたぐ時、最初ページ以外ではオーバーフローが解除されても認識がされずに、長体率のリミットいっぱいまでノンストップで長体をかけてしまう」不具合がありました。

前回の2つのスクリプトを合体して、書き直してみました。<b>boosterValue</b>という変数をいじることで、「七歩進んで〜」モードになります。速いかどうかはデータ次第なので、乱用注意。

![/images/2010/09/600-booster_mode_memo.jpg](/images/2010/09/600-booster_mode_memo.jpg)

#### 100107(thu)1130頃追記

kamisetoさんのコメントのコードを後半の方に転機、だんぜん速いです。

```js

/*
選択したセルだけオーバーフロー解除v2
"remove overflows on selected cells indesign, with booster"
使い方：表組の選択したセルだけをオーバーフロー解除します。
boosterValue = 1だと、1%刻みで長体処理をかけていきます。
boosterValueをいじることで、n%刻みで、かけすぎたら1%づつ戻す長体処理をします。
セルの縦組、横組によって水平垂直比率を切り替えます。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
//いじるならここ
var limitPercent = 30;//長体率の最小値％
var boosterValue = 3;//1で1%刻みで、それ以上は「n%刻み、かかりすぎたら1%戻す」処理
//ここまで
if(app.selection.length == 1){
  var selObj = app.selection[0];
  //表組の全選択か部分選択か
  if(selObj.constructor.name == "Table" || "Cell"){
    removeOVTable(selObj.cells);
  }
}

function removeOVTable(targetObj){
  for (var i = 0; i < targetObj.length; i++){
    var txtObj = targetObj[i].texts[0];
    //セルの組み方向が横組のとき
    if(targetObj[i].writingDirection == HorizontalOrVertical.HORIZONTAL){
      while (targetObj[i].overflows == true){
        txtObj.horizontalScale　-=　boosterValue;
        targetObj[i].label = "ov";//長体処理をしたところにスクリプトラベル
        targetObj[i].recompose();
        if(txtObj.horizontalScale <= limitPercent){
          break;
        }
      }
      //n%モードのとき、長体処理が終わった時の長体率をおぼえておく
      //1%モードのときはすぐにぬける
      var tempHoliz = targetObj[i].texts[0].horizontalScale;
      if(targetObj[i].label == "ov"){
        while (targetObj[i].overflows == false){
          txtObj.horizontalScale++;
          targetObj[i].recompose();
          if(txtObj.horizontalScale > (tempHoliz+boosterValue)){
            break;
          }
        }
        txtObj.horizontalScale--;
        targetObj[i].label = "";
      }
    }//if horizontal
    else{//セルの組み方向が縦組のとき
      while (targetObj[i].overflows == true){
        txtObj.verticalScale　-=　boosterValue;
        targetObj[i].label = "ov";
        targetObj[i].recompose();
        if(txtObj.verticalScale <= limitPercent){
          break;
        }
      }
      var tempVerti = txtObj.verticalScale;
      if(targetObj[i].label == "ov"){
        while (targetObj[i].overflows == false){
          txtObj.verticalScale++;
          targetObj[i].recompose();
          if(txtObj.verticalScale > (tempVerti+boosterValue)){
            break;
          }
        }
        txtObj.horizontalScale--;
        targetObj[i].label = "";
      }
    }//else vertical
  }//for target length
}//function
```

<b>090901(tue)2000頃追記</b>
参考までに、セル数292で、長体率が60〜100％くらいのデータでためした結果は以下のとおり

```js
---------------------
boosterValue is 10
157680
---------------------
boosterValue is 5
145780
---------------------
boosterValue is 3
202939
---------------------
boosterValue is 1
434275
---------------------
```

<b>100107(thu)1130頃追記</b>

kamisetoさんのコメントのコード転機、だんぜん速いです。

```js
function removeOVTable(targetObj){
  for (var i=0 , L = targetObj.length; i < L; i++){
    var txtObj=targetObj[i].texts[0];
    //縦か横か
    var HorV = targetObj[i].writingDirection == HorizontalOrVertical.HORIZONTAL ? 'horizontalScale'  : 'verticalScale';
    //オバーフロー
    if(targetObj[i].overflows == true){
      //まずは7%引いてみる
      txtObj[HorV] -= 7;
      targetObj[i].recompose();
      //オーバーフローが解消するまで16%づつ引く
      while(targetObj[i].overflows == true){
        txtObj[HorV] -= 16;
        targetObj[i].recompose();
      }
      //オーバーフローになるまで3%づつ足していく
      while(targetObj[i].overflows == false){
        txtObj[HorV] +=3;
        targetObj[i].recompose();
      }
      //オーバーフローが解消するまで1%づつ引く
      while(targetObj[i].overflows == true){
        txtObj[HorV]--;
        targetObj[i].recompose();
      }
    }
  }
};
```