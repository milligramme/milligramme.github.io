+++
title = "文字列とセルスタイル置換"
date = "2009-05-11T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2950](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2950) 
 
お題に挑戦。

セルの内容によって、セルスタイルを適用する。

白文字は段落スタイルで。

- "a"→セルスタイル"a"
- "b"→セルスタイル"b"
- "c"→セルスタイル"c"
- それ以外→"??"

switch〜case〜defaultで該当箇所以外をセルスタイル[なし]にしたいがうまくいかない？どうやって定義するのだろう？

<strike>後で調べる。</strike>わかった。

```js
/**
セル内容によってセルスタイルを置換する
"apply cellStyle by cell content"
使い方：
キーワードとそれに対応するセルスタイル予め設定しておき
セルを選択し実行する。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
var docObj=app.documents[0];
var noCellStyle=docObj.cellStyles.itemByName("[なし]");
var tblObj=app.selection;
var rowObj=tblObj[0].rows;
for (var ro=0; ro < rowObj.length; ro++){
    var cellObj=rowObj[ro].cells;
    for (var ce=0; ce < tblObj[0].columns.length; ce++){
        switch(cellObj[ce].contents){
            case "a": cellObj[ce].appliedCellStyle="a";break;
            case "b": cellObj[ce].appliedCellStyle="b";break;
            case "c": cellObj[ce].appliedCellStyle="c";break;
            default : cellObj[ce].appliedCellStyle=noCellStyle;
        }
    }
}
```