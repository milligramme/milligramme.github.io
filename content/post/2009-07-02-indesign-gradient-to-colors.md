+++
title = "グラデーション解体"
date = "2009-07-02T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
 [以前](http://www.milligramme.cc/wp/2009/06/indesign_11.html) にカラーチップからグラデーションを作ってみたので、今度はグラデーションストップに解体してみた。

<b>■使用前</b>

![/images/2010/09/507-gradient_splitter_before.jpg](/images/2010/09/507-gradient_splitter_before.jpg)

<b>■使用後</b>

![/images/2010/09/510-gradient_splitter_after.jpg](/images/2010/09/510-gradient_splitter_after.jpg)

グラデーション以外は処理しません。

```js
/*
選択したグラデーションオブジェクトを分解する。
"split colors from gradient objects"
使い方：
グラデーション塗りのオブジェクトを選択して実行します。
構成するグラデーションストップに分解してカラーチップを作成します。
カラーチップ内にはグラデーションストップの位置が段落スタイルを使って記載されます。

注意：
グループには未対応。
分解したカラーが新規スウォッチとして追加されません。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
//分解チップの大きさを設定
var chipSize=15;
var docObj=app.activeDocument;
//ルーラー原点をバックアップして「スプレッドに」
var rulerBk=docObj.viewPreferences.rulerOrigin;
var rulerTemp=1380143983;
docObj.viewPreferences.rulerOrigin=rulerTemp;
if(docObj.selection.length < 1){
  alert("塗りがグラデーションのオブジェクトを一つ以上選択して")
  docObj.viewPreferences.rulerOrigin=rulerBk;
  }
else{
  var selObj=docObj.selection;
  for(var se=0; se < selObj.length; se++){
    if(selObj[se].fillColor.constructor.name!="Gradient"){
      alert("塗りがグラデーションではありません");
      docObj.viewPreferences.rulerOrigin=rulerBk;
    }
    else{
      //後で調整できるようにラベルの段落スタイルをあてる
      var pStyleName="gradientStop_l  ocation_label";
      try{
        var pStyle=docObj.paragraphStyles.add({name:pStyleName});
      }catch(e){}
      var pStyle=docObj.paragraphStyles.itemByName(pStyleName);
      //段落スタイルの仮設定
      with(pStyle){
        try{
          appliedFont=app.fonts.item("Kozuka Gothic Pro");
        }catch(e){}
        try{
          fontStyle="M";
        }catch(e){}
        pointSize="6pt";
        leading="8pt";
        fillColor=docObj.colors.item("Black");
        strokeColor=docObj.colors.item("Paper");//白フチ文字にする
        strokeWeight="0.5pt";
      }
      //元のオブジェクトの左下座標
      var selx1=selObj[se].visibleBounds[1];
      var sely2=selObj[se].visibleBounds[2];
      var gradiObj=selObj[se].fillColor;
      var gradiStp=gradiObj.gradientStops;
      for(var i=0; i < gradiStp.length; i++){
        var stopColr=docObj.textFrames.add();
        //元のオブジェクトの左下辺を基準に分解チップを作成
        stopColr.visibleBounds = [sely2, selx1 + chipSize*i, sely2 + chipSize, selx1 + chipSize * i + chipSize];
        stopColr.fillColor=gradiStp[i].stopColor;
        stopColr.contents="loc:"+gradiStp[i].location.toFixed(1)+"%";
        //グラデーションストップの位置のラベルに段落スタイルをあてる
        if(parseFloat(app.version) < 5){
        //CS2用
          stopColr.paragraphs[0].applyStyle(pStyle);
        }else{
        //CS3用
          stopColr.paragraphs[0].applyParagraphStyle(pStyle);
        }
      }
    }
  }
}
//ルーラーを戻す。
docObj.viewPreferences.rulerOrigin=rulerBk;
```