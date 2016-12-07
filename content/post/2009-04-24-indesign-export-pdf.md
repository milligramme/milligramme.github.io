+++
title = "ここからここまでPDF書き出し"
date = "2009-04-24T00:00:00+09:00"
tags = ["extendscript", "indesign", "pdf"]

outdated = true
+++

ページ物で、「ページ範囲を任意で、たとえば　"1-3", "4", "5-8", "9", "10-12"。名前もそれぞれ、連番でなくバラバラな任意のもの」というPDFを個別に書きたいとき用に。

■こんな感じに

![/images/2010/09/353-each_pdf_out1.png](/images/2010/09/353-each_pdf_out1.png)

■ページ物があったとさ

![/images/2010/09/356-each_pdf_out2.png](/images/2010/09/356-each_pdf_out2.png)

■PDF名にしたいテキストをいれる。レイヤーを非表示でも、プリント属性で「プリントしない」にしてもいいかも

![/images/2010/09/359-each_pdf_out3.png](/images/2010/09/359-each_pdf_out3.png)

■PDF書き出し先フォルダを選択、やめたければここでキャンセル。

![/images/2010/09/362-each_pdf_out4.png](/images/2010/09/362-each_pdf_out4.png)

■スクリプトラベル分のPDFができる。中身は...

![/images/2010/09/365-each_pdf_out5.png](/images/2010/09/365-each_pdf_out5.png)

```js
/*
ここからここまでバラPDF書き出し
"export PDFs with my range"
使い方：
PDF名にしたい内容が書いてあるテキストフレームにスクリプトラベルをいれておきます。
次のスクリプトラベルが発生するまでの範囲でPDF書き出していきます。
PDF書出しプリセット「Sample」を使用しますので適当に変更してください。
milligramme
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
(function (){
  if(app.documents.length!=0){
    var docObj=app.documents[0];
    var pageObj=docObj.pages;
    var myPDFexPreset=app.pdfExportPresets.item("Sample");
    //開始・終了位置とPDF名
    var startP=new Array();
    var endP=new Array();
    var nameP=new Array();
    for(var i=0 ; i < pageObj.length; i++){
      //スクリプトラベルのあるテキストフレームが開始位置の配列
      var scrL=pageObj[i].textFrames.itemByName("kompeto");
      if(scrL == null){
        continue;
        }
      else {
        nameP.push(scrL.contents.replace(":","_")+".pdf");
        startP.push(i+1);
        endP.push(i+1);
      }
    }
    //ページが連続する箇所の終了位置を設定
    //前にずらして、1ひく、最後に最終ページを追加。
    endP.shift();　
    for(var k=0; k < endP.length; k++){
      endP[k] = endP[k]-1
    }
    endP.push(pageObj.length);
    //PDF書き出し。
    var myFolder = Folder.selectDialog("Choose a Folder to export");
    if(myFolder != null){
      for(j=0; j < startP.length ; j++){
        //"1-3"など書き出し範囲を設定、単ページも"6-6"となるけど大丈夫みたい。
        var pRange=startP[j]+"-"+endP[j];
        app.pdfExportPreferences.pageRange=pRange;
        var myFilePath=myFolder+"/"+nameP[j]
        var myFile=new File(myFilePath);
        docObj.exportFile(ExportFormat.pdfType,myFile,false,myPDFexPreset);
      }
    }
  }
  else{
    alert("open a document and try again");
  }
})();
```