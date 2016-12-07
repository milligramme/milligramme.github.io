+++
title = "トップレベルのグループをさがす"
date = "2010-07-23T00:00:00+09:00"
tags = ["extendscript", "illustrator"]
+++
InDesignのスクリプト書きに慣れていると、たまにIllustrator用に書こうとするとえらい苦労をします。

今回は、Illustratorではページアイテムの取得したいのに入れ子のアイテムもひっくるめて全部返してくるので、選択ツール（黒い方）で選択したような戻り値を期待しても取得できないのに手間取ったのですが、達人たちはこんな時「選択アイテム限定」とすることで制御してるらいいのですが。

とりあえずグループアイテムを選択ツールで選択したときの数(app.selection.length)と同等のアイテムが取得できないものか試したときのメモ。

グループアイテムを何個かまとめてグループ化したもの→groupItems.lengthだと入れ子たちもふくまれちゃう。

→これで一個になってほしい。

**条件**

- 親がレイヤー
- 子に何かしらのアイテムがある（一個でグループの場合も含む）

こんなのでいいのかな？　

![/images/2010/09/773-groups_on_doc.png](/images/2010/09/773-groups_on_doc.png)

![/images/2010/09/774-result_of_group_check.png](/images/2010/09/774-result_of_group_check.png)

トップレベルのグループアイテムを取得する。

```js
#target "Illustrator"
var doc = app.documents[0];
var topG = getTopGroup(doc.groupItems);
alert("top level group:"+topG.length+ "\r" +
  "PageItems:" + doc.pageItems.length + "\r" +
  "PathItems:" + doc.pathItems.length + "\r" +
  "CompoundPathItems:" + doc.compoundPathItems.length + "\r" +
  "GroupItems:" + doc.groupItems.length + "\r" +
  "Selection:" + doc.selection.length);
/**
* get top level group items
* @param {Array} group Array of GroupItem
* @returns {Array} tgr Array of top level GroupItem
*/
function getTopGroup (group) {
  var tgr=[];
  for (var i=0; i < group.length; i++) {
    //parent is Layer
    if(group[i].parent.typename == "Layer"){
      //group is formed with single path item
      if(group[i].pathItems.length == 1){
        tgr.push(group[i]);
      }
      //group is formed with compound path item
      if(group[i].compoundPathItems.length == 1){
        tgr.push(group[i]);
      }
      //group include any page items(textframe, pathitem)
      else if(group[i].pageItems.length != 0){
        tgr.push(group[i]);
      }
    }
  }
  return tgr;
}
```