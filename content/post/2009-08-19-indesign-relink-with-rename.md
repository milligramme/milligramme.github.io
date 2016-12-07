+++
title = "リネームして再リンク"
date = "2009-08-19T00:00:00+09:00"
tags = ["indesign"]
+++
InDesignの配置画像リネーム用のスクリプトを書いていたら、途中でほぼ同じようなスクリプトをみつけたので、参考にしてみた。

[InDesignupdate: Rename/Relink Script](http://avondale.typepad.com/indesignupdate/2005/09/renamerelink_sc.html) 
 
1つだけ選択したオブジェクトのリンクを変更して再リンクするというものですが、1つのファイルをいろんなところに配置しているとリンク切れが発生します。

加えて、とりあえず２点の処理を追加してみる。

![/images/2010/09/587-replace_memo.jpg](/images/2010/09/587-replace_memo.jpg)

- リネーム前と同じファイル名が他の箇所でも使用している場合、選択したオブジェクト以外のリンクも変更して更新するようにする。
- リネームしたい名前が既に存在している場合、ダイアログで、リネームはしないで既に存在するファイルへリンクを変更させるか、処理をキャンセルするか？　確認できるようする。ただし他に同名ファイルがあってもそれらはいじらない。［これはブックのパッケージやいろんな場所から同名リンクになっている配置画像をパッケージするとき、同名のファイルがだぶらないように、photo1.psdやphoto11.psdなどと名前が変わってしまう、リンク増殖事件に対応するため。］

### ファイルを直接操作するので注意してください。

```js
/*
Rename and Relink
"選択した配置画像のファイル名をリネームして再リンク"
■他にもリネーム前の同名のファイルがある場合
同階層であれば、それらもまとめて再リンク。ただし
同名でも階層が違う（リンク切れをしない）場合はそのまま。
■リネームしたいファイル名が既に存在する場合
そのファイルにリンクを変更（置換）するか、処理をキャンセルするか
を選択してから処理します。
動作確認：OS10.4.11 InDesign CS2, CS3
milligramme(mg)
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
if(app.documents.length != 0){
  var selObj = app.selection;
  if(selObj.length == 1){
    //選択しているものの判別
    switch(selObj[0].constructor.name){
      case "Rectangle":
      case "Oval":
      case "Polygon":
        if(selObj[0].allGraphics.length != 0){
          myRename(selObj[0].allGraphics[0]);
        }
        break;
      case "Image":
      case "EPS":
      case "PDF":
      case "PICT":
      case "ImportedPage":
        myRename(selObj[0]); break;
      case "Group":
        alert("グループ化されてます。\rグループ解除して一つだけ選択、または、ダイレクト選択してください")
    }//switch selection
  }//if selection.length
}//if document length

function myRename(sel){
  //現在の配置画像のファイルパスとファイル名
  var currentLink = sel.itemLink;
  var currentFileName = currentLink.name;
  var currentLinkFile = File(currentLink.filePath);
  //ダイアログにてリネームする
  var nwFileName=prompt ("rename", currentFileName, "RenameTo");
  //キャンセル押したときはリネームしない
  if(nwFileName != null){
    var nwLinkFile = currentLinkFile.rename(nwFileName);
    if(nwLinkFile == true){
      currentLink.relink(currentLinkFile);
      currentLink.update();
    }
    else{
      //リネームしたいファイル名のファイルが存在している
      //リネームせずに終了
      var replaceCheck = confirm("そのファイル名は既に存在しています。\r「はい/YES」ならそのファイルに置換、「いいえ/NO」なら処理をキャンセルします");
      //既に存在しているファイル名に
      if(replaceCheck == true){
        var existLinkFilePath = currentLink.filePath.replace(currentFileName,nwFileName);
        var existLinkFile = File(existLinkFilePath);
        currentLink.relink(existLinkFile);
        currentLink.update();
      }//if replace check
    }//else
  }//if prompt ok

  //リンク情報全体を取得、ほかに同名ファイルがあるかどうか調べる。
  var allLinks = app.documents[0].links;
  for(var i = allLinks.length-1; i >= 0; i--){
    if(allLinks[i].name == currentFileName){
      var otherLink = allLinks[i].parent.itemLink;
      //リンクが切れたらファイル名を置換、切れなかったらそれは無視=別階層
      if(allLinks[i].status == LinkStatus.LINK_MISSING){
        otherLink.relink(currentLinkFile);
        otherLink.update();
      }//if missing link
    }//if same filename
  }//for i
}//function myRename
```