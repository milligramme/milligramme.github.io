+++
title = "セル内の段落でセル分割"
date = "2009-07-08T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3316](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=3316) 

をみて、逆にセル内の段落でさらにセルを分割したかったのを思い出した。

ほぼ自分使用用。

```js
/*
表組内のセルを段落単位に分割
1段落=1セルに分割したいセルを1つ選択して実行。
複数セルには未対応（1セルだけ）
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
var selObj=app.selection[0];
if(selObj.constructor.name ==  "Cell"){
  //選択セルを憶えておく
  var cellPosition=selObj.cells[0].name;
  var cellPo=cellPosition.split(":");
  var poX=parseInt(cellPo[0]);
  var poY=parseInt(cellPo[1]);
  //後で作る一つ下のセル
  poY=poY+1;
  var newPosition=""+poX+":"+poY;
  paraObj=selObj.paragraphs;
  if(paraObj.length > 1){
    for(var pr=paraObj.length-1; pr >= 1; pr--){
      //行を下に追加
      var newCell=selObj.rows.add(LocationOptions.after,selObj);
      //最後の段落から随時移動
      paraObj[-1].move(LocationOptions.after,selObj.parent.cells.itemByName(newPosition));
      //最初に選んだセルを選択し直し
      app.selection=selObj.parent.cells.itemByName(cellPosition);
      selObj=app.selection[0];
      selObj.characters[-1].remove();
    }
  }
}
```

メモ）セル選択状態でを下に行を追加した時は、それも「合わせて選択状態」になる。

上に追加だと「合わせて選択状態」にはならない。

セル選択ではなく、中のテキスト選択やカーソル挿入状態では、どちらに挿入しても「合わせて選択状態」にならない。


<b>090709Thu1230ころ修正</b>

初期セル位置のXYが逆だったのを修正