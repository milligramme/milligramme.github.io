+++
title = "反転した水平垂直比率を逆転させる"
date = "2009-07-13T00:00:00+09:00"
tags = ["illustrator", "extendscript"]
+++

そろそろIllustratorのJavaScript（ExtendScript）もと思っている頃に、
「流用して下さい」といわれ、支給されたIllustratorファイルの文字サイズまわりが、
一部悲しいことになっていた（PDF経由で再保存のAi？）ので、とっかかりにAi用スクリプトを書いてみる。

しかし、こういうのも長体というのだろうか？

![/images/2010/09/529-scaling_heitai.png](/images/2010/09/529-scaling_heitai.png) ![/images/2010/09/530-scaling_chotai.png](/images/2010/09/530-scaling_chotai.png)

- 左：なぜか、変な長体。水平垂直比率が逆転？
- 右：垂直比率100％に固定、フォントサイズと水平比率で調整、自動行送りを解除。

IllustratorとInDesignとでオブジェクトモデルが似ていて非なる部分が多々あり戸惑う。

とりあえずは、色々書いて試す。

それからだんだんお互いの違いを理解した方が良さそう。

OSXのウィンドウキャプチャ（command+shift+4+space）ではCS3からのパネル<strike>（パレット）</strike>のタブがキャプチャーされないんだ！と途中で気付いた。

結構不便。

```js
/*
文字の垂直比率が100％以外を調整
"reset font vertical scale to 100"
使い方：
垂直比率が100%以外になったテキストを水平比率とフォントサイズで調整します。
テキストパス、フレームを選択して実行。
注意：
横組仕様です。
自動行送りは強制的に解除します。
エリアテキストの場合、ベースライン位置がずれます。
グループ化されたテキストには未対応、ダイレクト選択すれば大丈夫かも。
動作確認：OS10.4.11 Illustrator CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
(function(){
  var selObj = activeDocument.selection;
  //テキスト選択でないとき
  if(selObj.typename != "TextRange"){
    for(var i = 0; i < selObj.length; i++){
      //グループ化されてないとき
      if(selObj[i].typename != "GroupItem"){
        var paraObj = selObj[i].paragraphs;
        for(var j = 0; j < paraObj.length; j++){
          //垂直比率が100%以外ならば、
          if(tmpVe != 100){
            //初期値
            var tmpSz = paraObj[j].size;
            var tmpHo = paraObj[j].horizontalScale;
            var tmpVe = paraObj[j].verticalScale;
            var tmpLd = paraObj[j].leading;
            if(paraObj[j].autoLeading == true){
              paraObj[j].autoLeading = false;
            }
            paraObj[j].size = tmpSz * tmpHo / 100 * tmpVe / tmpHo;
            paraObj[j].horizontalScale = 100 * tmpHo / tmpVe;
            paraObj[j].verticalScale = 100;
            //行送りが自動なら解除して初期値をあてる
            paraObj[j].leading = tmpLd;
          }
        }
      }
    }
  }
})();
```