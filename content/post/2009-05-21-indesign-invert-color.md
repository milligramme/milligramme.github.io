+++
title = "ネガポジテキスト"
date = "2009-05-21T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

次のJavaScriptのお題として、InDesignSecrets [Split Text Color from Black to White in the Middle of a Frame](http://indesignsecrets.com/split-text-color.php) から「背景画像の境界で、上にのせたテキストの白黒を切り替えたいとき（かなり意訳）」

1. 背景の画像をコピーして、「元の位置にペースト」
1. ペーストした画像をダイレクト選択で削除、グラフィックフレームがのこる。
1. グラフィックフレームの塗りと線をなしにしておく。
1. 上にのせたテキストフレームをコピー。
1. 画像を削除したグラフィックフレームへ、テキストフレームを「選択範囲内にペースト」
1. テキストを入れ子にしたグラフィックフレームを前面にする。
1. 入れ子になったテキストの色を変える。

をやってみたいと思います。


090522 [せうぞーさんところ](http://d.hatena.ne.jp/seuzo/20090522/1242977738) でご指摘があったので一部スクリプトに加筆しました。

```js
/**
画像の上のテキストを反転させる
"reverse B/W color of texts over graphic"
使い方：
テキストフレームとグラフィックフレームを各１つ選択して実行下さい。
グラフィックフレームにかぶった部分のテキストカラーの
[黒]またはK=100、[紙色]またはC=0,M=0,Y=0,K=0を相互に切り替えます。
とりあえず、白黒のみの対応です。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
(function(){
  var docObj=app.activeDocument;
  var selObj=docObj.selection;
  if(selObj.length == 2){
    if(selObj[0].constructor.name!=selObj[1].constructor.name){
      for (var i=0; i < selObj.length; i++){
        if(selObj[i].contentType == ContentType.textType){
          var dupT=selObj[i].duplicate();
        }
        if(selObj[i].contentType == ContentType.graphicType){
          //グラフィックフレームを複製して中身を消去
          var dupG=selObj[i].duplicate();
          dupG.graphics[0].remove();
          dupG.fillColor="None";//塗りなしに
          dupG.strokeWeight = 0;//線をなしに
          //または
          //dupG.strokeColor = "None";
          dupG.bringToFront();
        }
      }
      dupT.select();
      app.cut();
      dupG.select();
      app.pasteInto();
      var dupPara=dupG.textFrames[0].paragraphs;
      for (var p=0; p < dupPara.length; p++){
        var dupTxt=dupPara[p].texts;
        for (tx=0; tx < dupTxt.length; tx++){
          switch (dupTxt[tx].fillColor.name){
            case "C=0 M=0 Y=0 K=100":
              dupTxt[tx].strokeWeight=0.05;
              dupTxt[tx].strokeColor="Paper";
              dupTxt[tx].fillColor="Paper"; break;
            case "Black":
              dupTxt[tx].strokeWeight=0.05;
              dupTxt[tx].strokeColor="Paper";
              dupTxt[tx].fillColor="Paper"; break;
            case "C=0 M=0 Y=0 K=0": dupTxt[tx].fillColor="Black"; break;
            case "Paper": dupTxt[tx].fillColor="Black";
          }
        }
      }
    }
    else{alert("テキストフレームとグラフィックフレームを各１つ選択して下さい。");}
  }
  else{alert("2つ選択して下さい。");}
})();
```