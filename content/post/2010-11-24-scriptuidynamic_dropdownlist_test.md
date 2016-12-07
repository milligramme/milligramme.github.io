+++
title = "Dynamic DropDownList Test"
date = "2010-11-24T00:00:00+09:00"
tags = ["scriptui"]
+++
Adobe Forums InDesign Scripting のダイナミックラジオボタンとドロップダウンリストというのを試してみた。

I tried this ==>  [Adobe Forums: [JS] - Dynamic RadioButtons and...](http://forums.adobe.com/thread/756052) 

The dropdown list isn't enable while related radiobutton isn't clicked.

I knew radiobuttons and dropdownlists must be wrapped with containar (group, panel), use "orientation", "alignChild" to layout,<del datetime="2010-11-24T03:56:34+00:00"> and callback not works in for loop expression. </del> add other approach script by  [kamiseto](http://d.hatena.ne.jp/kamiseto/)  and  [Loic Aigon](http://www.loicaigon.com/en/)  

ドロップダウンリストは関連したラジオボタンの選択状態によって使用可能になるテスト。

以前に for ループで作成したオブジェクトの扱いに困った件は一度配列に入れてあげることで解決出来る、あとレイアウトは基本 undefined のおまかせにして、orientation と alignChild で並べる。<del datetime="2010-11-24T03:56:34+00:00">コールバックに関してはそれぞれ作らないとうまくいかないので for でまわすと一番最後のコールバックのみが有効になるみたい。</del>（2010-11-24 コールバックをそれぞれつくらなくてもいい方法 kamisetoさんよりコメントを参照）

2010-11-25  [kamiseto](http://d.hatena.ne.jp/kamiseto/)  さんと  [Loic Aigon](http://www.loicaigon.com/en/)  さんのコメントのスクリプトを転記

![/images/2010/11/dynamic_dd_select1.png](/images/2010/11/dynamic_dd_select1.png)

![/images/2010/11/dynamic_dd_select3.png](/images/2010/11/dynamic_dd_select3.png)

my original script

```js
/**
 * Dynamic Radiobuttons and Dropdownlists TEST 
 * 
 * 2010-11-24 mg
 */

//value_arr[i][0] = for dropdownlist label
//value_arr[i][1] = for dropdownlist value's array
var value_arr = [ ["first",[1,2,3,4]], ["second",[5,6,7,8]], ["third",[9,'a','b','c']], ["fourth",['d','e','f']]];

var win = new Window('dialog', "Dynamic Dlg");
win.pnl = win.add('panel');
win.pnl.orientation = "row";
var rd_grp = win.pnl.add('group');
rd_grp.orientation = "column";
rd_grp.alignChild = "left";
//array for radiobutton
var r_arr = [];
for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
  r_arr.push(rd_grp.add('radiobutton', [0,0,90,23], value_arr[ri][0].toUpperCase()));
};
var dd_grp = win.pnl.add('group');
dd_grp.orientation = "column";
//array for dropdownlist
var d_arr = [];
for (var di=0, diL=value_arr.length; di < diL ; di++) {
  d_arr.push(dd_grp.add('dropdownlist', [0,0,90,23],  value_arr[di][1]));
  d_arr[di].selection = 0;//default selection
  d_arr[di].enabled = false;
};

//callback (must be defined each event?)
r_arr[0].onClick = function () {
  if (r_arr[0].value == true) {
    for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
      d_arr[ri].enabled = false;//switch all dropdownlist to false
    };
    d_arr[0].enabled = true;
  };
}
r_arr[1].onClick = function () {
  if (r_arr[1].value == true) {
    for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
      d_arr[ri].enabled = false;
    };
    d_arr[1].enabled = true;
  };
}
r_arr[2].onClick = function () {
  if (r_arr[2].value == true) {
    for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
      d_arr[ri].enabled = false;
    };
    d_arr[2].enabled = true;
  };
}
r_arr[3].onClick = function () {
  if (r_arr[3].value == true) {
    for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
      d_arr[ri].enabled = false;
    };
    d_arr[3].enabled = true;
  };
}

win.show();
```

**script by kamiseto**

use curry to set relation radiobutton and dropdownlist

```js

//リストが増えた場合、こう書いといた方が楽じゃないかな。
//定型的な処理なら簡単に使い回しもできますし。
//value_arr[i][0] = for dropdownlist label
//value_arr[i][1] = for dropdownlist value's array
var value_arr = [
["first",[1,2,3,4]],
["second",[5,6,7,8]],
["third",[9,'a','b','c']],
["fourth",['d','e','f']],
["555",['g','h','i']],
["666",['j','k','l']],
["777",['m','n','o']],
["888",['p','q','r']],
["999",['s','t','u']]
];

var win = new Window('dialog', "Dynamic Dlg");
win.pnl = win.add('panel');
win.pnl.orientation = "row";
var rd_grp = win.pnl.add('group');
rd_grp.orientation = "column";
rd_grp.alignChild = "left";
//array for radiobutton
var r_arr = [];

//Curry
Function.prototype.curry = function(){
  var slice = Array.prototype.slice, args = slice.apply(arguments) , that = this;
    return function (){
      return that.apply(this, args.concat (slice.apply(arguments)));
    };
};

//
var r_click_evet = function (i) {
  if (r_arr[i].value == true) {
    for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
      d_arr[ri].enabled = false;//switch all dropdownlist to false
    };
    d_arr[i].enabled = true;
  };
}

for (var ri=0, riL=value_arr.length; ri < riL ; ri++) {
  r_arr.push(rd_grp.add('radiobutton', [0,0,90,23], value_arr[ri][0].toUpperCase()));

  //callback (must be defined each event?) ==> no
  r_arr[ri].onClick = r_click_evet.curry(ri);

};
var dd_grp = win.pnl.add('group');
dd_grp.orientation = "column";
//array for dropdownlist
var d_arr = [];
for (var di=0, diL=value_arr.length; di < diL ; di++) {
  d_arr.push(dd_grp.add('dropdownlist', [0,0,90,23], value_arr[di][1]));
  d_arr[di].selection = 0;//default selection
  d_arr[di].enabled = false;
};

win.show();
```

**by Loic Aigon** (I add a bit code and comment)

use eventlistener in panel, grouping same id's radiobutton and dropdownlist to compare

```js
Application.prototype.main=function(){
  dlg();
}

function dlg(){
  var w = new Window('dialog','Dynamic Dropdowns',undefined);
  var p1 = w.add('panel',undefined, "Pick something");
  var rb1 = new DD(p1, "DD 1", ["a","b","c"], true, true,"rb1");
  var rb2 = new DD(p1, "DD 2", ["1","2","3"],false,false,"rb2");
  var rb3 = new DD(p1, "DD 2", ["x","y","z"],false,false,"rb3");
  var rb4 = new DD(p1, "DD 2", ["Ho","Ha","He"],false,false,"rb4");

  p1.addEventListener('click', resetDD, false);

  function resetDD(e){
    if(e.target.constructor.name=="RadioButton"){
      try{
        var pnl = e.target.parent.parent;//panel parentparent of rdbutton
        var toid = e.target.oid;
        e.target.value=true;
        for(var i=0; i < pnl.children.length; i++){
          var gp = pnl.children[i];//group is parent of panel
          var rb = gp.children[0];//set same oid, has same parent
          var dd = gp.children[1];//set same oid, has same parent
          if(rb.oid != toid){
            rb.value=false;
            dd.enabled=false;
          }
        }
      }
      catch(e){

      }
    }
  }
  w.show();
}

function DD(parentObj, radiobuttonLabel, listItems, rbValue, DDEnable,rbOid){
  var gp = parentObj.add('group');
  gp.orientation='row';
  gp.alignChild='left';//set alignment
  var rb = gp.add('radiobutton', [0,0,90,23],radiobuttonLabel);//set bound
  rb.value = rbValue ||　false;
  rb.oid=rbOid;
  var dd = gp.add('dropdownlist', [0,0,90,23], listItems);//set bound
  dd.selection = 0;
  dd.enabled=DDEnable || false;

  rb.onClick = function(){
    if(this.value==false){
      dd.enabled=false;
    }
    else{
      dd.enabled=true;
    }
  }

}

app.main();
```