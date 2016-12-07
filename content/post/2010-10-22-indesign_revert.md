+++
title = "復帰させるメソッド revert（）"
date = "2010-10-22T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
InDesign で自動処理の途中で一度「復帰」してリセットした後に、なにか別な処理を継続的にしたいときに、今まではドキュメントのファイルパスを記憶して **閉じて**、そのファイルパスのドキュメントを **開いて** 再開していた。さっきちらっとDOMヘルプで調べものしてたら、CS3 から **revert()** なんて便利なものが追加されてる……知らなかった。

ふつうに `app.documents[0].revert()` などと使うと、確認のダイアログがでますので `UserInteractionLevel` で出ないようにしておくといいかも。

![/images/2010/10/revert_dialog.png](/images/2010/10/revert_dialog.png)

**revert()での復帰の処理はこんな感じ**

```js
//
//なんか処理
//ドキュメントをいじり倒す
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.NEVER_INTERACT;

var rvt = app.documents[0].revert();

app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;
//リセットしてから引き続き
//なんか処理
//
```

返り値のBooleanは

- 復帰処理できる保存されていない状態　rvt = **true**
- 保存されている状態　rvt = **false**
- 一度も保存されていない場合　 rvt = **false**

となるようです。

**21:15ころ追記**
<del datetime="2010-10-22T12:17:24+00:00">よくよく見てみるとDocument 以外にも、Story, TextFrame, PageItem, Group, Oval, Rectangle, Polygon, GraphicLine などにも使えるようです。</del>

試したらDocument以外のオブジェクトに実行しても false を返して復帰されないようです、残念。

**19:00ころ追記**
CS2で使用すると無限ループ的反応無しの状態になっちゃうのでバージョンで分岐した方が確実。

**今まではこんなことしてた**

```js
if(app.documents.length != 0){
  var docObj = app.documents;
  //一度閉じるドキュメントのパスを保管する配列
  var docTemp = new Array();
  //保存されていない場合の保存処理
  for (var idc=docObj.length-1; idc >=0 ; idc--) {
    if(docObj[idc].saved == false){
      work = Folder.selectDialog("choose folder to save document \'"+docObj[idc].name+"\'");
      if(work){
        var docPath = work+"/"+docObj[idc].name.replace(/$/,".indd");
        docObj[idc].save( new File(docPath) );
      }
      else{
        alert("precess was canceled");
        exit();
      }
    }// end of save?
    else{
      var docPath = docObj[idc].fullName;
    }
    docTemp.push (docPath);
  }// end of for doc-1st

  for (var idc=docObj.length-1; idc >=0 ; idc--) {
    //
    //なんか処理
    //    
    //保存せずに閉じてまた開く
    docObj[idc].close(SaveOptions.NO);
    }// end of for doc-2nd
  }

//一度閉じたドキュメントをまた開く
for (var i=0; i < docTemp.length; i++) {
  //モーダルダイアログ対策で一時的にアラートとダイアログ非表示
  app.scriptPreferences.userInteractionLevel = UserInteractionLevels.NEVER_INTERACT;
  app.open(File(docTemp[i]));
  app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;
}
    //
    //なんか処理
    //    

```

