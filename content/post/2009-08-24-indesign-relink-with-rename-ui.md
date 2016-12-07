+++
title = "リネームして再リンク（インターフェイス付き）"
date = "2009-08-24T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
インターフェイスが活きなければ、無理して付いてなくてもよいかもという話。

以前に書いてみたInDesignの配置画像リネーム用のスクリプト [[InDesign]リネームして再リンク](/2009/08/19/indesign-relink-with-rename.html) であらかじめ、

わかっていることがあるなら（同名ファイルがあったときや、リネーム後のファイル名が既にあるとき）何度もダイアログを出すよりも、

最初から対処すればよいかもと、ダイアログの練習がてらにインターフェイスをつけてみることにしてみた。

![/images/2010/09/589-replace_option_dialog.jpg](/images/2010/09/589-replace_option_dialog.jpg)

うーん。使い勝手はどうだろう。

一番目のチェックボックスはチェック外して、あえてリンクを切るという意味があるかどうか？

二番目のチェックボックスのデフォルトがオンでよいものか？

やっぱり確認ダイアログの方でよい気がします。

スクリプトの処理系で、実行したらほっとく系スクリプトの途中で、ダイアログがぽこぽこ挿入されると嫌だけど、

手作業の延長系スクリプトだと、作業の流れ（リズム）にのっていれば途中の確認ダイアログもそんなに気にならないような気がします。

自分は今回のスクリプトだと後者。まー、こういう失敗も経験ということで。

```js
/*
Rename and Relink with Dialog option
"選択した配置画像のファイル名をリネームして再リンク
チェックボックスのインターフェイス付き"
■他にもリネーム前の同名のファイルがある場合
同階層であれば、それらもまとめて再リンク［デフォルト］。ただし
同名でも階層が違う（リンク切れをしない）場合はそのまま。
※チェックボックスのチェックを外すと同名ファイルはリンク切れになります。
■リネームしたいファイル名が既に存在する場合
そのファイルにリンクを変更（置換）する［デフォルト］。
上記の項目をどうするか
ダイアログで予めをオプションを選択してから処理します。
動作確認：OS10.4.11 InDesign CS3
milligramme(mg)
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
if(app.documents.length!=0){
  var selObj=app.selection;
  if(selObj.length == 1){
    //選択しているものの判別
    switch(selObj[0].constructor.name){
      case "Rectangle":
      case "Oval":
      case "Polygon":
        if(selObj[0].allGraphics.length!=0){
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
  var currentLink=sel.itemLink;
  var currentFileName=currentLink.name;
  var currentLinkFile=File(currentLink.filePath);
  //ダイアログにてリネームする
  var replaceDialog=app.dialogs.add({name:"リネームしてリリンク！", canCancel: true});
  with(replaceDialog){
    with(dialogColumns.add()){
      with(borderPanels.add()){
        with(dialogColumns.add()){
          staticTexts.add({staticLabel: "現在のファイル名です。リネームしてください。"});
          var ReplaceFilenameField = textEditboxes.add({editContents: currentFileName, minWidth: 320});
        }
      }
      with(borderPanels.add()){
        with(dialogColumns.add()){
          //ダイアログのデフォルトはここをいじる。falseにすると他ファイルはリンク切れします
          var renameOtherChecboxControlsGroup = checkboxControls.add({staticLabel: "他にリネーム前と同名ファイルがある時、一緒に処理する", checkedState:true});
        }
      }
      with(borderPanels.add()){
        with(dialogColumns.add()){
          //ダイアログのデフォルトはここをいじる。falseにするとリンクの置き換えをしません
          var relinkToExistChecboxControlsGroup = checkboxControls.add({staticLabel: "ファイル名が既に存在している時、それにリンクを置換する", checkedState:true});
        }
      }
    }
    //ダイアログの入力結果
    if(replaceDialog.show() == true){
      //新しいファイル名は
      var nwFileName= ReplaceFilenameField.editContents;
      //オプション：他に同名があるとき
      if(renameOtherChecboxControlsGroup.checkedState == true){
        var renameSameOthers=1;
      }
      //オプション：リネーム後の名前がすでに存在しているとき
      if(relinkToExistChecboxControlsGroup.checkedState == true){
        var relinkToExist=1;
      }
    replaceDialog.destroy();
    }
    else{
      replaceDialog.destroy();
    }
  }

  //キャンセル押したときはリネームしない
  if(nwFileName!=null){
    var nwLinkFile=currentLinkFile.rename(nwFileName);
    if(nwLinkFile == true){
      currentLink.relink(currentLinkFile);
      currentLink.update();
    }
    else{
      //リネームしたいファイル名のファイルが既に存在している。オプションtrueのときリンク置換
      if(relinkToExist == 1){
        var existLinkFilePath=currentLink.filePath.replace(currentFileName,nwFileName);
        var existLinkFile=File(existLinkFilePath);
        currentLink.relink(existLinkFile);
        currentLink.update();
      }//if replace check
      else{alert("そのファイル名は既に存在しています。リンクを変更しません。")}
    }//else
  }//if dialog ok

  //リンク情報全体を取得、ほかに同名ファイルがあるかどうか調べる
  //オプションtrueのとき他の同名ファイルもリネームして再リンク
  if(relinkToExist == 1){
    var allLinks=app.documents[0].links;
    for(var i=allLinks.length-1; i >= 0; i--){
      if(allLinks[i].name == currentFileName){
        var otherLink=allLinks[i].parent.itemLink;
        //リンクが切れたらファイル名を置換、切れなかったらそれは無視=別階層
        if(allLinks[i].status == LinkStatus.LINK_MISSING){
          otherLink.relink(currentLinkFile);
          otherLink.update();
        }//if missing link
      }//if same filename
    }//for i
  }
}//function myRename
```