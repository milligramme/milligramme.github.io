+++
title = "セルの素性を知る"
date = "2009-06-11T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
表がたっぷりある●●●報告書などで、表のフォーマットが統一されているか？

表のセルの大きさなどを確認したいとき、流用元データがいろんなところの寄せ集めで統一感がないのをなんとか把握したいとき、InDesign CS3以降ならば、表スタイル・セルスタイルでコントロールできる内容もありますが、それでも高さや幅については、基本的にはいちいち選択して確認しなくてはなりません。

（たぶん）実用性をおいておいて、兎に角かいてみようと、JavaScriptでかいてみました。

allPageItemとpageItemの違いを知れたのが今回の収穫。

何するスクリップトか？選択したオブジェクトに表があれば、複製してセル内にセルの幅と高さ大きさを表示します。

応用したらもっといろいろ情報を詰め込めるかも。

### ■表まじりのドキュメント

![/images/2010/09/463-cell_info1.jpg](/images/2010/09/463-cell_info1.jpg)

### ■表を含んだテキストフレームを複製して、表の部分にセルの情報を書き込みます。

![/images/2010/09/467-cell_info2.jpg](/images/2010/09/467-cell_info2.jpg)

### ■ミリ・ポイント換算誤差がでます。セルの高さを最小限度にしている時には、おしりにアスタリスクがつくようにしてます。（一番左のセルのみ）

![/images/2010/09/470-cell_info3.jpg](/images/2010/09/470-cell_info3.jpg)

```js
/*
選択したオブジェクト内の表セルの縦横サイズを表示する。
"add labels of cell information over tables"
使い方：
オブジェクトを選択して実行します。
表のあるテキストフレームを複製して、セルの情報を書込みます。
注意：
アンカー付きオブジェクト内の表は無視されます。
ポイント・ミリ誤差で端数が出る場合があります。
結合セルは最初のセルの値になります。
他にも何かあるかもしれません。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
(function(){
  var docObj=app.activeDocument;
  //ルーラー原点をバックアップして「スプレッドに」
  var rulerBk=docObj.viewPreferences.rulerOrigin;
  var rulerTemp=1380143983;
  docObj.viewPreferences.rulerOrigin=rulerTemp;
  //情報書き込み用レイヤーをつくっておく
  try{
    var dupLy=docObj.layers.add({name:"TableInfo"});
  }catch(e){}
  dupLy=docObj.layers.itemByName("TableInfo");
  var selObj=docObj.selection;
  for(var j=0; j < selObj.length; j++){
    //グループ化してないテキストフレームなら
    if(selObj[j].constructor.name == "TextFrame"){
      var dupTf=selObj[j].duplicate();
      dupTf.move(dupLy);
      TableInfo();//セル内処理部分へ
    }//if texframe
    //グループ化してるとき
    else if(selObj[j].constructor.name == "Group"){
      var grpItem=selObj[j].allPageItems;
      for (k=0; k < grpItem.length; k++){
        if(grpItem[k].constructor.name == "TextFrame"){
          dupTf=grpItem[k].duplicate();
          dupTf.move(dupLy);
          TableInfo();//セル内処理部分へ
        }//if texframe
      }// for k
    }// if group
  }//for j
  //ルーラーを戻す。
  docObj.viewPreferences.rulerOrigin=rulerBk;
  //セル内処理部分
  function TableInfo(){
    //表があったら処理
    if(dupTf.tables[0]!=null){
      var dupTbl=dupTf.tables;
      for(var h=0; h < dupTbl.length; h++){
        var cellObj=dupTbl[h].cells;
        for(var i=0; i < cellObj.length; i++){
          //ポイントミリ誤差が気になるなら、小数点第3位で四捨五入
          //var hg=cellObj[i].height.toFixed(3);
          //var hg=cellObj[i].width.toFixed(3);
          var hg=cellObj[i].height;
          var wd=cellObj[i].width;
          //書込み内容を指定
          var cellLab="w:"+wd+", h:"+hg;
          //セルの高さが最小限度ならアスタリスクをつける
          if(cellObj[i].autoGrow == true){
            cellObj[i].autoGrow=false;
            cellObj[i].contents=cellLab+"*";
          }
          else{
            cellObj[i].contents=cellLab;
          }
          //情報の書式などを調整、指定外はスタイル継承
          with(cellObj[i]){
            fillColor="Paper";
            paragraphs[0].pointSize="4pt";
            paragraphs[0].leading="6pt";
            paragraphs[0].fillColor="Black";
            paragraphs[0].strokeColor="None";
          }//for  with
        }//for i
      }//for h
    }
    else{dupTf.remove();}//if table not null
  }//function Tableinfo
  alert("おわった");
})();
```