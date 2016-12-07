+++
title = "CS3での結合セルの変な挙動"
date = "2010-02-25T00:00:00+09:00"
tags = ["indesign"]
+++
InDesignCS2とCS3での .merge() 系スクリプトの挙動が変だったので、結合セルについてちょっと調べてみたら、CS3ではセルの結合の仕方・選択の仕方によって cells.length の値が変になる。下図のシアンの塗り部分を選択した場合 (constructor.name == "Cell") 、CS3は2を返す。全体を選択する (constructor.name == "Table") とちゃんと5を返す。CS2はちゃんと3と5を返す。
色が塗られた選択中のセルの数を1セル目に書くスクリプトで、Mac OSX10.4.11 CS2, CS3で確認。CS3のバグかな、CS4、CS5はいかに？
せうぞーさんのエントリー「 [セルの選択でクラッシュ](http://d.hatena.ne.jp/seuzo/20090115/1232010131) 」の不具合はこれに起因するのかな？

![/images/2010/09/730-merge_cells_cs2.jpg](/images/2010/09/730-merge_cells_cs2.jpg)

![/images/2010/09/731-merge_cells_cs3.jpg](/images/2010/09/731-merge_cells_cs3.jpg)

```js
//結合セルのセル数チェック
var docObj=app.documents.add();
var tfObj=docObj.textFrames.add({geometricBounds:[20,20,270,70]});
var insertTf=tfObj.insertionPoints[-1];
var matx={bodyRowCount:3, columnCount:2}
var tableObj1=insertTf.tables.add(matx);
with(tableObj1){
  cells[3].merge(tableObj1.cells[5]);
  rows[1].fillColor="Cyan";
  rows[2].fillColor="Cyan";
  rows[1].select();
  rows[2].select(SelectionOptions.ADD_TO);
  cells[0].contents=""+app.selection[0].cells.length;//CS2だと3、CS3だと2?
}
var tableObj2=insertTf.tables.add(matx);
with(tableObj2){
  cells[2].merge(cells[4]);
  rows[1].fillColor="Magenta";
  rows[2].fillColor="Magenta";
  rows[1].select();
  rows[2].select(SelectionOptions.ADD_TO);
  cells[0].contents=""+app.selection[0].cells.length;//3
}
var tableObj3=insertTf.tables.add(matx);
with(tableObj3){
  cells[4].merge(cells[5]);
  rows[1].fillColor="Yellow";
  rows[2].fillColor="Yellow";
  rows[1].select();
  rows[2].select(SelectionOptions.ADD_TO);
  cells[0].contents=""+app.selection[0].cells.length;//3
}
var tableObj4=insertTf.tables.add(matx);
with(tableObj4){
  cells[2].merge(cells[3]);
  rows[1].fillColor="Yellow";
  rows[2].fillColor="Yellow";
  rows[1].select();
  rows[2].select(SelectionOptions.ADD_TO);
  cells[0].contents=""+app.selection[0].cells.length;//3
}
```