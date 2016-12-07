+++
title = "セル幅をそれぞれ増減"
date = "2009-07-27T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
InDesignの表組の調整についてセルの高さは、「指定値を使用」・「最小限度」を組み合わせてある程度コントロールできるのですが、セルの幅は指定値での指定しかできません。

たとえば、それぞれのセルの幅が違っていたり、互い違いな幅に続いていたりしている状態で、それぞれのセル幅を1mm小さくするとか大きくするという風にしたい時。

まとめて選択すると入力欄がホワイトアウト（入力できるけど混在状態）してしまうので、それぞれのセルを選択して数値をいじらなくてはなりません。

そうなったらイヤなのでスクリプトを書いてみました。

### 幅の違うセルを5(mm)増やしてみる

![/images/2010/09/564-cell_width.gif](/images/2010/09/564-cell_width.gif)

```js
/*
表組内のセル幅をまとめて増減
"increase or decrease each cells width"
表組全体または一部セルを選択して実行。
結合したセルもそれぞれのセル幅が増減します。
単位は環境設定に依存。
動作確認：OS10.4.11 InDesign CS3
milligramme(mg)
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
var n=prompt("セル幅の増減値を入力して下さい。","-1");
//ダイアログが煩わしい時は直接、数値を設定。
//var n=-1;
if(n!=null){
  var incdec=eval(n);
  var docObj=app.activeDocument;
  var selObj=app.selection[0];
  switch (selObj.constructor.name){
    case "Table" :
      TableResize(selObj,incdec) break;
    case "Cell" :
      CellResize(selObj,incdec) break;
    default : alert("テーブル又はセルを選択して下さい。");
  }
}

//選択範囲が表組全体のとき
function TableResize(selObj, incdec){
  var rwObj=selObj.rows;
  var colmObj=rwObj[0].columns;
  for(var i=0; i < colmObj.length;i++){
    var cW=colmObj[i].width;
    cW+=incdec;
    colmObj[i].width=cW;
  }
}

//選択が範囲がセルのとき
function CellResize(selObj, incdec){
  //選択している行・列
  var rwObj=selObj.rows;
  var colmSel=selObj.columns;
  //選択している最初のセルの位置
  var firstcell=selObj.cells[0].name;
  var startcell=firstcell.split(":");
  //列はstartcell[0]、行はstartcell[1]から始まる
  var j=startcell[1];
  var rowSel=selObj.parent.rows[j];
  for(var i=startcell[0]; i < colmSel.length+eval(startcell[0]); i++){
    var cW=rowSel.columns[i].width;
    cW+=incdec;
    rowSel.columns[i].width=cW;
  }
}

```