+++
title = "グループなのにグループじゃない（IllustratorからD&D）"
date = "2009-06-26T00:00:00+09:00"
tags = ["indesign", "illustrator"]
+++
メモメモ
InDesignCS2に、IllustratorCS2からドラッグアンドドロップしたオブジェクトは、複数の場合、問答無用でグループ化された状態になっていて、フォントもグラフィック化されています。そして、オーバープリントの白問題を引き連れてきます。<u>そのオブジェクト</u>は、constructor.nameでみると"Group"。でも、allPageItems.length で数えても「1つのオブジェクト」としか認識しません。グループ解除してから数えようとすると InDesign が強制終了してしまいます。何か触れてはいけないものを触っているのでしょうか？

チェックの仕方

Illustratorからドラッグアンドドロップする組合せ、それぞれオーバープリント有効にしてます。

### ■四角と文字
上記の「そのオブジェクト」というのは吹き出しと文字みたいな組合せで試しました。

![/images/2010/09/498-dd_overprint_white_moji2kaku.png](/images/2010/09/498-dd_overprint_white_moji2kaku.png)

**■文字と文字**
Illustratorのナマ文字からInDesignでグラフィック化された文字が含まれていると、数は合わなくなるみたい。グラフィック化された文字だと、 constructor.name が "Group" で、allPageItems.length が「ゼロ」なのです。グループなのに所属人数がゼロ、部活なら廃部です。JavaScriptさんは人数分の処理したいのにゼロなので処理しようとしないのです。そして、これをグループ解除して処理しようとするとInDesignが落ちます。

![/images/2010/09/497-dd_overprint_white_moji2moji.png](/images/2010/09/497-dd_overprint_white_moji2moji.png)

### ■四角と四角
この組合せだとInDesignのパスたちと同じ扱いができるみたいで allPageItems の数は合うのですが、これを塗りか線を白のオーバープリントにすると、InDesignが落ちます。[紙色] のオーバープリントはInDesignではありえないことなのです。

![/images/2010/09/499-dd_overprint_white_kaku2kaku.png](/images/2010/09/499-dd_overprint_white_kaku2kaku.png)

### ■わかった事

1. テキストをドラッグアンドドロップするとグループなのに構成員がゼロになる。
1. スクリプトは白（[紙色]）のオーバープリントを解除するタイミングで落ちる。

オーバープリントを解除する JavaScript を書いてみたのですが、InDesign純正パーツでないと落ちます。InDesign では [紙色] にオーバープリントはかからないので、Illustratorからの改造パーツを扱えないとそもそもあまり意味がないのです。

InDesign CS3とIllustrator CS3 でも確認してみても同様でした。ついでにスクリプトを加筆、[紙色] にオーバープリントがかかった状態で、無理矢理オーバープリントを解除しようとすると、落ちるので、一度[黒]にして[紙色]にもどす（勝手に外れる）ようにした。Illustrator からの文字は相変わらず処理できず......

ものかのさんのコメントより <del datetime="2010-10-05T02:11:34+00:00">app.swatches.item("None")</del> の箇所4箇所をdocObj..swatches.item("None") に書き換えました。

```js
/*
選択したオブジェクトの線と塗りのオーバープリントを解除する。
注意
Illustratorからドラッグアンドドロップしたアウトライン化していない文字は
うまく処理されません。InDesignが強制終了する場合があります。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
(function(){
  var docObj=app.activeDocument;
  var selObj=docObj.selection;
  for(var i=0; i < selObj.length; i++){
    if(selObj[i].constructor.name == "Group"){
      var grpItm=selObj[i].allPageItems;
      //alert(grpItm.length);
      for(var j=0; j < grpItm.length; j++){
        if(grpItm[j].fillColor!=docObj.swatches.item("None")){
          if(grpItm[j].fillColor.name == "Paper"){
            //白以外にして白に戻す
            grpItm[j].fillColor="Black";
            grpItm[j].fillColor="Paper";
          }
          else{
            grpItm[j].overprintFill=false;
          }
        }// if fill color
        if(grpItm[j].strokeColor!=docObj.swatches.item("None")){
          if(grpItm[j].strokeColor.name == "Paper"){
          //白以外にして白に戻す
            grpItm[j].strokeColor="Black";
            grpItm[j].strokeColor="Paper";
          }
          else{
            grpItm[j].overprintStroke=false;
          }
        }//if stroke color
      }//for group item
    }//if group
    else{
      if(selObj[i].fillColor!=docObj.swatches.item("None")){
        if(selObj[i].fillColor.name == "Paper"){
          //白以外にして白に戻す
          selObj[i].fillColor="Black";
          selObj[i].fillColor="Paper";
        }
        else{
          selObj[i].overprintFill=false;
        }
      }// if fill color
      if(selObj[i].strokeColor!=docObj.swatches.item("None")){
        if(selObj[i].strokeColor.name == "Paper"){
          //白以外にして白に戻す
          selObj[i].strokeColor="Black";
          selObj[i].strokeColor="Paper";
        }
        else{
          selObj[i].overprintStroke=false;
        }
      }// if stroke color
    }// else not group
  }
})();
```