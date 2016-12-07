+++
title = "ダイアログ外骨格をInDesignでつくる"
date = "2010-01-19T00:00:00+09:00"
tags = ["indesign", "scriptui"]
+++

どちらかというと自家用です。

InDesignのドキュメント上でScriptUIのダイアログをレイアウトしてみるスクリプト。

元のアイデアは、  [Open Space](http://www.openspc2.org/)  古旗さんの「組版時間を半減する! InDesign自動処理実例集（ [amazon](http://www.amazon.co.jp/dp/4774136875/) ）」の自動GUI生成ツール（P190）と、net でたまたま見かけた GUI をつくるswfファイル （ [http://www.jkozniewski.com/tools/csib_loader.swf](http://www.jkozniewski.com/tools/csib_loader.swf) ）のインターフェイス。これらを参考に味付けしてます。

ドキュメントの単位系をポイントに設定し、スウォッチ名にはScriptUIのパーツたちの名前をあてたドキュメントを用意。

（サンプル：<a href='/images/2010/09/703-dlg_maker_cs3.indd_.zip'>-dlg_maker_cs3.indd.zip</a>）

![/images/2010/09/705-dlg_maker_swatch_as_object.jpg](/images/2010/09/705-dlg_maker_swatch_as_object.jpg)

そこに「UIオブジェクト色」のテキストフレームをレイアウトしていきます。

オブジェクトの種類によって、内容テキストの扱いが変わります。

（スライダーはパラメーター、ドロップダウンリストはリスト配列、あとはラベルのように）

ペーストボード上のオブジェクトは無視します。

現時点で、すべてのオブジェクトはダイアログウィンドウ内にベタ並べ状態なので、パネルとグループの内側のオブジェクトも階層的に内包されてません（親子関係にない）。

実行するとESTKのJava Scriptコンソールに、ダイアログのソースを吐き出し、ダイアログ表示を実行します。okかcancelボタンのクリックで（またはenterかescキーで）ダイアログを閉じます。

![/images/2010/09/704-dlg_maker_dialog.jpg](/images/2010/09/704-dlg_maker_dialog.jpg)

```js
/**
InDesignでScriptUIダイアログをつくる
"creat UI outline"
使い方：
スウォッチ名にUI オブジェクトのタイプ名をあてたドキュメントの
1ページ目にレイアウトされたテキストフレームを対象に実行すると
ScriptUIダイアログを作成します。
PB上のオブジェクトは無視します。
注意：
'dialog'のテキストフレームは左上原点開始つくらないと表示がずれます。
動作確認：OS10.4.11 InDesign CS3
milligramme
www.milligramme.cc
*/
var textFrm=app.documents[0].pages[0].textFrames;
var uiText="";
var bodyText="";
var headText="";
var containerText="";
//右端と下端を入れておく配列
var lefPos=new Array();
var botPos=new Array();
for(var i=0; i < textFrm.length; i++){
  var propText=textFrm[i].contents;
  var colNam=textFrm[i].fillColor.name; // スウォッチ名がUIオブジェクトのタイプ
  var gBonText="["+textFrm[i].geometricBounds[1]+", "+textFrm[i].geometricBounds[0]+", "+textFrm[i].geometricBounds[3]+", "+textFrm[i].geometricBounds[2]+"]";
  lefPos.push(textFrm[i].geometricBounds[3]);
  botPos.push(textFrm[i].geometricBounds[2]);
  switch(textFrm[i].fillColor.name){
    //ダイアログ
    case "dialog":
      headText="var dlg=new Window('"+colNam+"', '"+propText+"' , "+gBonText+");\r" ;break;
    //パネルとグループ、現状では親として役割を果たさない枠的存在
    case "panel":
      containerText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"');\r"; break;
    case "group":
      containerText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"');\r"; break;
    //それぞれテキスト内容を反映させる
    case "statictext":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"' , {multiline : true});\r"; break;
    case "edittext":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"' , {multiline : false});\r"; break;
    case "radiobutton":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"');\r"; break;
    case "checkbox":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"');\r"; break;
    //直接arrayやvalueを入れる
    case "dropdownlist":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , "+propText+");\r"; break;//propTextは配列または空の配列[ ]
    case "slider":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , "+propText+");\r"; break;//propTextは現在値, 最小値, 最大値

    //ボタン、okとcancelは別格
    case "button_ok":
      bodyText+="dlg.add('button' , "+gBonText+" , '"+propText+"' , {name: 'ok'});\r"; break;
    case "button_cancel":
      bodyText+="dlg.add('button' , "+gBonText+" , '"+propText+"' , {name: 'cancel'});\r"; break;
    case "button":
      bodyText+="dlg.add('"+colNam+"' , "+gBonText+" , '"+propText+"');\r"; break;
    default : break;
  }
}
//ダイアログをセンターに持ってくるのと表示用テキスト
var centerText="dlg.center();\r";
var showText="dlg.show();";
//ダイアログ用のテキストフレームがなかったとき用
if(headText == ""){
  lefPos.sort(function(a,b){return a < b});
  //max = lefPos[0];
  botPos.sort(function(a,b){return a < b});
  //max = botPos[0];
  headText="var dlg=new Window('dialog','untitled',[0,0,"+lefPos[0]+" , "+botPos[0]+"]);\r";
}
uiText=headText+centerText+containerText+bodyText+showText;
$.writeln(uiText);//ESTKのJavaScriptコンソールに書き出し
eval(uiText);//ダイアログ表示を実行
```