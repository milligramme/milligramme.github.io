+++
title = "選択したセルだけオーバーフロー解除"
date = "2009-04-23T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
セル選択したところだけをピンポンポイントでオーバーフロー解除したいとき。横組・縦組のセルにも対応。

テキストフレームを選択してはだめ。親テキストフレームを選択している時のエラー処理、かんがえる。

090901(tue)

連結テキストフレーム内に表組が連続していて、それがページをまたぐ時、最初ページ以外ではオーバーフローが解除されても認識がされずに、長体率のリミットいっぱいまでノンストップで長体をかけてしまう不具合がありました

[InDesign_オーバーフロー解除スクリプトたちを修正](/2009/09/01/indesign-modify-cancel-overflow.html) 

```js

/*選択したセルだけオーバーフロー解除
"remove overflows on selected cells"
使い方：表組の選択したセルだけをオーバーフロー解除します。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
(function (){
  var limitPer =50;
  var selObj = app.selection;
  if(selObj[0].constructor.name == "Table" || "Cell"){
    var cellObj = selObj[0].cells;
    for (var i=0; i < cellObj.length; i++){
      while (cellObj[i].overflows == true){
      var txtObj = cellObj[i].texts[0];
        if(cellObj[i].writingDirection == 1752134266){
          //Horizontal  1752134266   横組
          //Vertical  1986359924   縦組
          txtObj.horizontalScale--;
          selObj[0].parent.recompose();
          if(txtObj.horizontalScale <= limitPer){
            break;
          }
        }
        else{
          txtObj.verticalScale--;
          selObj[0].parent.recompose();
            if(txtObj.verticalScale<=limitPer){
              break;
            }
        }
      } // while
    } //for i
  } // if
})();
```