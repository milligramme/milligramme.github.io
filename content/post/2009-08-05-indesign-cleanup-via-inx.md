+++
title = "inxですっきりしてみる。"
date = "2009-08-05T00:00:00+09:00"
tags = ["indesign", "extendscript"]

outdated = true
+++
InDesign で.inddドキュメントがなんか怪しいことになっていてクリーニングしたいとき、

.inx で書出しをして、再度.indd にすることが多い（それが正しいかどうかはわかりませんが）。

CS2、CS3 では.inxファイルを開いても「名称未設定-○○」のような新規ドキュメント扱いになってしまい、まとめて.inx を開いた日には、どれがどれやら.......inx のあるフォルダを選択して、同階層に、同名の.indd ドキュメントをつくるスクリプト。

```js
/*
inxをinddに
"inx2indd"
.inxのあるフォルダを選択（サブフォルダは未対応）して実行。
同階層に同名の.inddを保存。
開かないで開く裏方処理。
動作確認：OS10.4.11 InDesign CS2,CS3
milligramme(mg)
www.milligramme.cc
*/
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
(function(){
  var inxFolder=Folder.selectDialog("inxを含むフォルダを選択");
  if(inxFolder == null){
    alert("フォルダが未選択");
    return;
  }
  //フォルダパスを取得
  var inxFpath=new File(inxFolder).fsName;
  var inxFlist=File(inxFpath).getFiles("*.inx");
  for(var i=0; i < inxFlist.length; i++){
    //.inxをhide状態で開く
    var docObj=app.open(inxFlist[i], false);
    var docName=inxFlist[i].name;
    docName=docName.replace(/\.inx$/,"");
    //名前をつけて別名保存
    docObj=docObj.save(new File(inxFpath+"/"+docName+".indd"));
    docObj.close(SaveOptions.no);
  }
})();
```