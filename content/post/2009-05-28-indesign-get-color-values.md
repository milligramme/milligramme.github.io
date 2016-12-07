+++
title = "何色か知るべきか否か"
date = "2009-05-28T00:00:00+09:00"
tags = ["extendscript", "indesign"]

outdated = true
+++

InDesign上のオブジェクトの色を知りたいときが、たまにあるかもしれません。そんなとき用のJavaScript。
一応CMYK用です。OSX 10.4.11　 InDesign CS2、CS3で確認。

![/images/2010/09/416-add_color_label1.jpg](/images/2010/09/416-add_color_label1.jpg)

■色付きオブジェクトにこんなラベルを付けます。

![/images/2010/09/419-add_color_label2.jpg](/images/2010/09/419-add_color_label2.jpg)

■ひとびとは [Celebrity No](http://www.dafont.com/search.php?psize=m&amp;q=celebrity) をグラフィック化したもの。

```js
/**
選択したオブジェクトの塗りのCMYK値を別レイヤーに書出します。
"add labels of color information over objects"
使い方：
オブジェクトを選択して実行します。
グループ化されているとダメ。
段落スタイルをあてるので、後でラベルのフォントなどを調整できます。
CMYKのプロセスカラー、特色に対応しているはず。
濃淡つきの色はそのCMYK値に変換されます。
「スウォッチ名 00％」という濃度付きスウォッチはスウォッチ名になります。
グラデーションはグラデーション名になります。
塗りなしは無視します。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
if(app.documents.length >= 1 && app.selection.length >= 1){
  var docObj=app.documents[0];
  var selObj=app.selection;
  //ストーリーを横書きに
  docObj.storyPreferences.storyOrientation=1752134266;
  //ルーラー原点をバックアップして「スプレッドに」
  var rulerBk=docObj.viewPreferences.rulerOrigin;
  var rulerTemp=1380143983;
  docObj.viewPreferences.rulerOrigin=rulerTemp;
  ////メイン処理
  main(docObj, selObj);
  //ルーラーを戻す。
  docObj.viewPreferences.rulerOrigin=rulerBk;
  alert("おわった");
}
else{exit();}

function main(docObj, selObj){
  //後で調整できるようにラベルの段落スタイルをあてる
  var pStyleName="swatch_value_label";
  var pStyle;
  try{
    pStyle=docObj.paragraphStyles.add({name:pStyleName});
  }catch(e){}
  pStyle=docObj.paragraphStyles.itemByName(pStyleName);
  //段落スタイルの仮設定
  with(pStyle){
    try{
      appliedFont=app.fonts.item("Kozuka Gothic Pro");
    }catch(e){}
    try{
      fontStyle="M";
    }catch(e){}
    pointSize="8pt";
    leading="9pt";
    fillColor=docObj.colors.item("Black");
    strokeColor=docObj.colors.item("Paper");//白フチ文字にする
    strokeWeight="0.2pt";
  }//with pstyle
  //ラベル用のレイヤー
  var labelLyr;
  try{
    labelLyr=docObj.layers.add({name:"cmyk label"});
  }catch(e){}
  labelLyr=docObj.layers.itemByName("cmyk label");
  for (var i=0; i < selObj.length; i++){
    //塗りが「なし」のときはスキップ
    if(selObj[i].fillColor.constructor.name!="Swatch"){
      var selYX=selObj[i].geometricBounds;
      var labelObj=docObj.textFrames.add(labelLyr);
      labelObj.contentType=ContentType.textType;
      labelObj.geometricBounds=selYX;
      //ラベルに回り込みを無視を設定
      labelObj.textFramePreferences.ignoreWrap=true;
      if(selObj[i].fillColor.constructor.name == "Gradient"){
        //グラデーションのときはグラデーション名
        labelObj.contents=selObj[i].fillColor.name;
      }
      else{
        var cmyk=selObj[i].fillColor.colorValue;
        //濃度があるとき（ただし継承「-1」を除く）
        if(selObj[i].fillTint!=-1){
          cmykTnt=selObj[i].fillTint;
          c="C=" +cmyk[0]*0.01*cmykTnt;
          m="M=" +cmyk[1]*0.01*cmykTnt;
          y="Y=" +cmyk[2]*0.01*cmykTnt;
          k="K=" +cmyk[3]*0.01*cmykTnt;
          labelObj.contents=c+"  [r]"+m+" [r]"+y+" [r]"+k;
        }
        else{//スウォッチ名の右一文字が「％」の濃度付きスウォッチのとき
          var nmLng=selObj[i].fillColor.name.length;
          if(selObj[i].fillColor.name.substring(nmLng-1,nmLng) == "%"){
            labelObj.contents=selObj[i].fillColor.name;
          }
          else{//濃度継承「-1」（カラー値そのもの？）のとき
            c="C=" +cmyk[0];
            m="M=" +cmyk[1];
            y="Y=" +cmyk[2];
            k="K=" +cmyk[3];
            labelObj.contents=c+"  [r]"+m+" [r]"+y+" [r]"+k;
          }
        }//elseスウォッチ名の右一文字が「％」
      }
      //ラベルに段落スタイルをあてる
      if(parseFloat(app.version) < 5){//CS2用
        labelObj.paragraphs[0].applyStyle(pStyle);
      }
      else{//CS3用
        labelObj.paragraphs[0].applyParagraphStyle(pStyle);
      }
    }//if 塗りが無し以外のとき
  }//for sel length
  if(parseFloat(app.version) < 5){//CS2用
    docObj.search(" [r]" ,false, false, false, false,"\r");
  }
  else{//CS3用
    app.findTextPreferences.findWhat="[r]";
    app.changeTextPreferences.changeTo="\r";
    docObj.changeText();
  }
}
```