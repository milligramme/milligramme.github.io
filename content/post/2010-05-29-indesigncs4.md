+++
title = "CS4以前で新規のレイヤーを任意の位置につくる"
date = "2010-05-29T00:00:00+09:00"
tags = ["indesign"]
+++
DTPの勉強部屋17のTL見ながら、久々にInDesignの豆スクリプト。そういえば、新規レイヤーは一番上にしかできなかったんだ。

```js
/*変更前*/ var targetLayer=doc.layers[lay.selectedIndex];
/*変更後*/ var targetLayer=doc.layers.itemByName(lname[lay.selectedIndex]);
```

![/images/2010/09/758-add_layer.png](/images/2010/09/758-add_layer.png)

```js
//InDesignのレイヤーを任意の位置に作る
if(app.documents.length!=0){
  var doc=app.documents[0];
  var lname=doc.layers.everyItem().name;
  var loc=['の前面','の背面', '最前面','最背面'];
  var locEnum=[1650812527,1634104421,1650945639,1701733408]
  //$.writeln(lname.toString().replace(/\,/g,"\r"));
  var d=app.dialogs.add({name:"add layer"});
  with(d){
    with(dialogColumns.add()){
      with(borderPanels.add()){
        with(dialogColumns.add()){
          staticTexts.add({staticLabel : "Add Layer"});
        }
        with(dialogColumns.add()){
          var lay=dropdowns.add({stringList : lname, selectedIndex:0});
        }
        with(dialogColumns.add()){
          var lis=dropdowns.add({stringList : loc, selectedIndex:0});
        }
      }
    }
  }
  if(d.show() == true){
    //  var layerName=Nam.editContents;
    var targetLayer=doc.layers.itemByName(lname[lay.selectedIndex]);
    var locOption=locEnum[lis.selectedIndex];
    var l=doc.layers.add();
    //  l.move(eval(locOption), targetLayer);//CS3だとエラーになる
    l.move(locOption, targetLayer);
    d.destroy()
  }
  else{
    d.destroy();
  }
}
```