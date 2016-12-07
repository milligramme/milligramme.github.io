+++
title = "黒以外のオーバープリントを解除"
date = "2010-01-08T00:00:00+09:00"
tags = ["illustrator", "extendscript"]
+++
Illustratorで **黒以外のオブジェクト** にかかったオーバープリントを解除します。

該当するオブジェクトがあれば、塗りごと、線ごとにダイアログに確認しながらオーバープリントを解除します。

孤立点とか、また別な問題があるドキュメントではエラーになる場合があります。そこまで面倒はみません。

```js
/*
黒以外のオーバープリント解除
"turn off overprint setting for non black object "
使い方：
実行すると塗り、線ごとにダイアログを出すのでオーバープリント解除するか決める。
動作確認：OS10.4.11 Illustrator CS3
milligramme
www.milligramme.cc
*/
var doc = documents[0];
var pgItm = doc.pageItems;
//塗り
app.selection = null;
for (var i = 0; i < pgItm.length; i++){
    //テキスト系の処理
    if(pgItm[i].typename == "TextFrame"){
        var fiTxtColor = pgItm[i].textRange.fillColor;
        if(pgItm[i].textRange.overprintFill == true){
            if (fiTxtColor.black == 100 
                && fiTxtColor.cyan == 0 
                && fiTxtColor.magenta == 0 
                && fiTxtColor.yellow == 0){
                continue;
            }
            else{
                pgItm[i].selected = true;
            }
        }
    }
    else{//それ以外のオブジェクト
        var fiColor = pgItm[i].fillColor;
        if(pgItm[i].fillOverprint == true){
            if (fiColor.black == 100 
                && fiColor.cyan == 0 
                && fiColor.magenta == 0 
                && fiColor.yellow == 0){
                continue;
            }else{
                pgItm[i].selected = true;
            }
        }
    }
}
removeOVPfill(app.selection);
//線
app.selection = null;
for (var i = 0; i < pgItm.length; i++){
    //テキスト系の処理
    if(pgItm[i].typename == "TextFrame"){
        var stTxtColor = pgItm[i].textRange.strokeColor;
        if(pgItm[i].textRange.overprintStroke == true){
            if (stTxtColor.black == 100 
                && stTxtColor.cyan == 0 
                && stTxtColor.magenta == 0 
                && stTxtColor.yellow == 0){
                continue;
            }
            else{
                pgItm[i].selected = true;
            }
        }
    }
    else{//それ以外のオブジェクト
        var stColor = pgItm[i].strokeColor;
        if(pgItm[i].strokeOverprint == true){
            if (stColor.black == 100 
                && stColor.cyan == 0 
                && stColor.magenta == 0 
                && stColor.yellow == 0){
                continue;
            }
            else{
                pgItm[i].selected = true;
            }
        }
    }
}
removeOVPstroke(app.selection);

//選択されたオブジェクトのオーバープリント解除（塗り）
function removeOVPfill(sel){
    if(sel.length == 0){
        return;
    }
    c = confirm ("remove fill OverPrint?", false, "ぬり");
    if(c == true){
        for(var i = 0; i < sel.length; i++){
            sel[i].fillOverprint = false;
            try{
                sel[i].textRange.overprintFill = false;
                }catch(e){}
            sel[i].selected = false;
        }
    }
}

//選択されたオブジェクトのオーバープリント解除（線）
function removeOVPstroke(sel){
    if(sel.length == 0){
        return;
    }
    c = confirm ("remove stroke OverPrint?", false, "せん");
    if(c == true){
        for(var i = 0; i < sel.length; i++){
            sel[i].strokeOverprint = false;
            try{
                sel[i].textRange.overprintStroke = false;
                }catch(e){}
            sel[i].selected = false;
        }
    }
}
```