+++
title = "七歩進んで一歩戻る"
date = "2009-06-12T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

以前の [InDesign選択したセルだけオーバーフロー解除](/2009/04/23/indesign-cancel-cell-overflows.html) で気になることがあって試してみたくなった。

### ちまちまひく（前回のスクリプト）

100％-1-1-1-1-1-1-1, -1-1-1-1-1-1-1, -1-1-1-1-1-1-1, -1-1-1-1-1-1-1, -1-1-1-1-1-1-1, -1-1-1=62％

![/images/2010/09/473-benchmark_c-1.png](/images/2010/09/473-benchmark_c-1.png)

### ひきすぎてちょっともどす（今回のスクリプト）

100％-7-7-7-7-7-7+1+1+1+1=62％

![/images/2010/09/3d3f079e21e1f23a2cac51cf6306628f.png](/images/2010/09/3d3f079e21e1f23a2cac51cf6306628f.png)

**090612Fri昼に追記**別な条件でも試してみる。

### ちまちまひく（前回のスクリプト）

100％-1-1=98％

![/images/2010/09/475-benchmark_98-1-1.png](/images/2010/09/475-benchmark_98-1-1.png)

### ひきすぎてちょっともどす（今回のスクリプト）

100％-7+1+1+1+1+1=98％

![/images/2010/09/0200ef12ef406c77f7dfbcf8e0be2da8.png](/images/2010/09/0200ef12ef406c77f7dfbcf8e0be2da8.png)

ぐっと長体掛けない場合には、逆に時間がかかります。三歩か五歩進んで一歩戻るくらいがよいのでしょうか？

**090901(tue)**

<blockquote>連結テキストフレーム内に表組が連続していて、それがページをまたぐ時、最初ページ以外ではオーバーフローが解除されても認識がされずに、長体率のリミットいっぱいまでノンストップで長体をかけてしまう不具合がありましたので、new
エントリにてスクリプトを書き直しました。</blockquote>
<blockquote> [InDesign_オーバーフロー解除スクリプトたちを修正](/2009/06/12/indesign-cancel-overflow.html/511) </blockquote>

```js
/*選択したセルだけオーバーフロー解除
"remove overflows on selected cells7+1"
使い方：
表組の選択したセルだけをオーバーフロー解除します。
動作確認：OS10.4.11 InDesign CS2、CS3
milligrammewww.milligramme.cc
*/

(function (){
    var limitPer =50;//リミット50％
    var selObj=app.selection;
    if(selObj[0].constructor.name == "Table" || "Cell"){
        var cellObj=selObj[0].cells;
        for (var i=0; i < cellObj.length; i++){
          var txtObj=cellObj[i].texts[0];
            while (cellObj[i].overflows == true){
                if(cellObj[i].writingDirection == 1752134266){
                    //Horizontal      1752134266      横組
                    //Vertical  1986359924      縦組
                    //初期長体率を記憶しておく、あとでつかう
                    var tempHoliz=txtObj.horizontalScale;
                    txtObj.horizontalScale-=7;
                    selObj[0].parent.recompose();
                    if(txtObj.horizontalScale <=limitPer){
                        break;
                    }
                }//if horizontal minus
                else{
                    var tempVerti=txtObj.verticalScale;
                    txtObj.verticalScale-=7;
                    selObj[0].parent.recompose();
                    if(txtObj.verticalScale <=limitPer){
                        break;
                    }
                }//if vertical minus
            }      //超体かけすぎたのを戻す。初期長体率を超えないように
            if(cellObj[i].writingDirection == 1752134266){
                while (cellObj[i].overflows == false){
                    txtObj.horizontalScale++;
                    selObj[0].parent.recompose();
                    if(txtObj.horizontalScale > tempHoliz){
                        break;
                    }
                }
                txtObj.horizontalScale--;
            }//if horizontal plus
            else{
                while (cellObj[i].overflows == false){
                txtObj.verticalScale++;
                selObj[0].parent.recompose();
                if(txtObj.verticalScale > tempVerti){
                    break;
                }
            }
        txtObj.verticalScale--;
        }//if vertical plus
    }
}})();
```