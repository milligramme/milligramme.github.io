+++
title = "ライブラリーに名付けて追加"
date = "2009-09-14T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
「名称未設定......嗚呼！！名称未設定」

![/images/2010/09/629-add_to_library.jpg](/images/2010/09/629-add_to_library.jpg)

InDesignドキュメント上のオブジェクトからライブラリーの「ページ上のアイテムを別々のオブジェクトとして追加」すると、
リンクファイル以外は「名称未設定」という名前になり、区別がつきません。

![/images/2010/09/630-store_to_lib_app_def.jpg](/images/2010/09/630-store_to_lib_app_def.jpg)

せめて名前はこうしたい↓

- テキストなら内容を要約する
- 空のテキストなら空っぽらしく
- 表組なら1セル目の内容
- グループおよび形状の区別をする
- フレーム枠の非表示状態で追加

![/images/2010/09/631-store_to_lib_with_profile.jpg](/images/2010/09/631-store_to_lib_with_profile.jpg)


```js
/*
ライブラリーに名前を付けてオブジェクトを追加
"store to library with name"
使い方：
選択中のオブジェクトを名前を付けて、ライブラリーに保存
動作確認：OS10.6.4 InDesign CS3
milligramme
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
if (app.selection.length > 0){
  //ライブラリーにフレーム枠の付かないように一時的にオフにする
  var tempFrameEdge = app.activeDocument.viewPreferences.showFrameEdges;
  app.activeDocument.viewPreferences.showFrameEdges = false;

  //ライブラリーがなければ、新規作成、キャンセルすれば終了
  if(app.libraries.length == 0){
    var indlPath = Folder.selectDialog("ライブラリの保存場所をきめてください");
    if (indlPath === null) {
      app.activeDocument.viewPreferences.showFrameEdges = tempFrameEdge;
      exit();
    }
    indlObjName = new File(indlPath + "/" + new Date().getTime() + ".indl")
    var indlObj = app.libraries.add(indlObjName);
    // alert("開いているライブラリーがありません");
  }
  var indlObj = app.libraries[0];
  var selObj = app.selection;
  main(indlObj, selObj);
  //フレーム枠の表示を元に戻す
  app.activeDocument.viewPreferences.showFrameEdges = tempFrameEdge;
}

function main (lib, sel) {
  var dscrTXT;//ライブラリーの詳細テキスト
  for(var i = 0; i < sel.length; i++){
    var selCnstName = sel[i].constructor.name;
    switch(sel[i].constructor.name){
      case "Group":
        lib.store (sel[i],{name:selCnstName} );
        break;
      case "TextFrame":
        if(sel[i].texts.length > 0){
          if(sel[i].texts[0].contents.length == 0){
            lib.store (sel[i], {name: "Empty " + selCnstName});
          }
          else{
            dscrTXT = sel[i].texts[0].contents;
            try{
              dscrTXT = sel[i].tables[0].cells[0].contents;
            }catch(e){}
            //内容のあたま16文字で名付ける
            lib.store (sel[i], { name:dscrTXT.substring(0, 16), description: dscrTXT });
          }
        }
        break;
      case "Rectangle":
      case "GraphicLine":
      case "Oval":
      case "Polygon":
        if(sel[i].graphics.length != 0){
          lib.store (sel[i], { description: "" + sel[i].graphics[0].itemLink.filePath });
        }
        else{
          lib.store (sel[i], {name:selCnstName});
        }
        break;
      default: continue;
    }
  }
}
```