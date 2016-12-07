+++
title = "EditTextとタブの関係"
date = "2010-07-14T00:00:00+09:00"
tags = ["scriptui"]
+++

ScriptUI のダイアログで EditText を `for ( )｛ ｝` 式で回してコンテンツに応じて生成しようとしたとき、入力時にタブで飛ばないことがあったのでメモ。

普通EditTextはタブでフィールドを移動できます。

違うnameの EditText であっても、生成順にタブ移動できるし、StaticText と EditEext を織り交ぜても大丈夫。

でも CheckBox, DropDownList, RadioButton が含まれるとダメ。

そんな時は、GroupやPanelで隔離すれば大丈夫になるので、別な `for ( )｛ ｝` 式で生成すればよいみたい。

![/images/2010/09/769-win_test12.png](/images/2010/09/769-win_test12.png)

```js
//タブで次のフィールドに飛ぶ
//edittext
var win = new Window('dialog',"edittext text",undefined);
var x=0;
for (var i=0; i < 5; i++) {
  win.add('edittext',[5,5+x,300,25+x],"1",{name:"first"});
  win.add('edittext',[35,25+x,300,45+x],"2",{name:"second"});
  x += 35;
};
win.add('button',undefined,"ok",{name:'ok'});
win.show();
//edittext + statictext
var win2 = new Window('dialog',"edittext + statictext test",undefined);
x=0;
for (var i=0; i < 5; i++) {
  win2.add('edittext',[5,5+x,300,25+x], i);
  win2.add('statictext',[5,25+x,300,45+x],"text-"+i);
  x += 35;
};
win2.add('button',undefined,"ok",{name:'ok'});
win2.show();
```

![/images/2010/09/770-win_test345.png](/images/2010/09/770-win_test345.png)

```js
//タブが効かない。
//edittext + statictext + checkbox
var win3 = new Window('dialog',"edittext + statictext + checkbox test",undefined);
x=0;
for (var i=0; i < 3; i++) {
  win3.add('edittext',[5,5+x,300,25+x],i);
  win3.add('checkbox',[5,25+x,300,45+x],"checbox-"+i);
  win3.add('statictext',[5,45+x,300,60+x],"text-"+i);
  x += 45;
};
win3.add('button',undefined,"ok",{name:'ok'});
win3.show();
//edittext + statictext + dropdownlist
var win4 = new Window('dialog',"edittext + statictext + dropdown test",undefined);
x=0;
for (var i=0; i < 3; i++) {
  win4.add('edittext',[5,5+x,300,25+x],i);
  win4.add('dropdownlist',[5,25+x,300,45+x],['a','b','c']);
  win4.add('statictext',[5,45+x,300,60+x],"text-"+i);
  x += 45;
};
win4.add('button',undefined,"ok",{name:'ok'});
win4.show();
//edittext + statictext + radiobutton
var win5 = new Window('dialog',"edittext + statictext + radiobutton",undefined);
x=0;
for (var i=0; i < 3; i++) {
  win5.add('edittext',[5,5+x,300,25+x],i);
  win5.add('radiobutton',[5,25+x,300,45+x]);
  win5.add('statictext',[5,45+x,300,60+x],"text-"+i);
  x += 45;
};
win5.add('button',undefined,"ok",{name:'ok'});
win5.show();
```

![/images/2010/09/771-win_test6.png](/images/2010/09/771-win_test6.png)

```js
//GroupやPanelで隔離すれば大丈夫になる。
//edittext + statictext + radiobutton
var win6 = new Window('dialog',"edittext + statictext + radiobutton",undefined);
x=0;
for (var i=0; i < 3; i++) {
  win6.add('edittext',[5,5+x,300,25+x],i);
  win6.add('statictext',[5,45+x,300,60+x],"text-"+i);
  x += 35;
};
var x2 = x + 10;
win6.group = win6.add('group',[5,x2,300,x2+130]);
var x3 = 0
for (var j=0; j < 3; j++) {
  win6.group.add('radiobutton',[5,25+x3,300,45+x3],"radiobutton-" + j);
  x3 += 30;
};
win6.add('button',undefined,"ok",{name:'ok'});
win6.show();
```