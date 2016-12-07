+++
title = "となりの空セルと結合"
date = "2009-05-05T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignの表組で階層状になったものをつくるとき用。

■使用前元ネタの表計算アプリから読み込んで、こんな状態になったのを、おとなりの空きセルを結合してすっきりしたい。

ちょっとぐらいなら手作業でもいいけど何百行もあるとちょっとイヤ。

処理する部分だけ選択して実行。

![/images/2010/09/385-merge_cells1.png](/images/2010/09/385-merge_cells1.png)

■使用後あとはちょちょっと罫線を調整したりしてみる。

![/images/2010/09/388-merge_cells2.png](/images/2010/09/388-merge_cells2.png)

090602 kamisetoさんのご指摘を受けて修正。

```js
/**
右となりの空セルと結合
"merge with empty neighbor cells"
使い方：
結合したい範囲を選択して実行。
選択したセルの右となりの列が空なら結合します。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
var selObj = app.activeDocument.selection;
//選択した列が2つ以上のときに処理する。
var colmLng = selObj[0].columns.length;
if(colmLng >= 2){
  var baseCell=selObj[0].cells[0].name.split(":");
  //セルの開始位置
  var baseCe=baseCell[0]-=0;//配列からゼロをひいて数値にする
  var rowObj=selObj[0].rows;
  for(var ro=0; ro < rowObj.length; ro++){
    for(var ce=colmLng+baseCe-1; ce > baseCe; ce--){
      if(selObj[0].rows[ro].cells[ce].contents  == ""){
        selObj[0].rows[ro].cells[ce].merge(selObj[0].rows[ro].cells[ce-1]);
      }
    }
  }
}
else{
  alert("Select 2 columns at least")
}
```