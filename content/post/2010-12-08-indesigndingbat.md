+++
title = "Dingbat入力パレットのひな形"
date = "2010-12-08T00:00:00+09:00"
tags = ["indesign", "scriptui", "extendscript"]
+++
InDesignで記号類を使っている文字スタイルで入力するとき用に試作したパレット。

下のドロップダウンメニューでドキュメント内にある文字スタイルを選ぶと、連動してボタン群の表示フォントが変わります（フォント指定無しならデフォルトのまま）。

ボタンを押すとテキスト選択位置に**その文字スタイル**でその文字を入力します。

字形パレットではスタイルまで出力できない。

![/images/2010/12/insert_with_style.png](/images/2010/12/insert_with_style.png)

### やってみて色々わかったことがあったのでメモ。

#### ボタン幅

CS3ではボタンの幅の最小値の制限から70pxより小さく出来ないので、列数をCS3とCS4以降で変えてます（CS3は5列、CS4以降は8列）。文字用の配列によってパレットの大きさは可変します。

#### 表示できる文字、できない文字

ボタンの表示は文字用の配列に、欧文[0-9A-Za-z]と記号類をいれているのですが、Unicodeの私用領域（0xf02dとか）にマッピングしてるフォントではボタンに反映されませんが入力はできるはずです（WebdingsとかWingdingsとか）。配列内はUnicodeの16進数表記でString.fromCharCode(xxx)と変換してからでもいいかも。

![/images/2010/12/use_private_area.png](/images/2010/12/use_private_area.png)

String.fromCharCode() で '0xf04a', '0xf04b', '0xf04c' を指定すると私用領域のも表示されるが、通常フォントでは文字化けしてる。

文字用の配列内で「\」「’」「”」はバックスラッシュでエスケープ、「&」はそのままだと表示されないので「&&」として書出しを一文字だけにするようにしてます。

#### 認識されるフォント

スタイルが Regular, Bold, Italic のフォントしかボタンに反映されません（Bold Italicも認識するらしいのですが、自分の環境だと認識しない）。合成フォントもダメ（スタイルが「-」になっちゃうから？）

#### その他

OSX 10.4ではうまく表示されないかもしれないです。（height:23px以上に大きくしてもアクアボタンのまま）

```js
/**
 *  Dingbats Input palette
 *  
 *  2010-12-01 mg
 */
#targetengine "session"

var button_pointsize = 14;//button font size
var value_arr = [
  ' ',
  String.fromCharCode('0xf04a'),
  String.fromCharCode('0xf04b'),
  String.fromCharCode('0xf04c'),
  'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','W','X','Y','Z',
  '[','\\',']','-','=','_','+',
  'a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','w','x','y','z',
  '{','|','}','~','!','@','#','$','%','^','&&','*','(',')',
  '1','2','3','4','5','6','7','8','9','0',
  ';',':','\'','\"',',','.','<','>','?','/',
];
if (app.documents.length !== 0) {
  var doc = app.documents[0];
  var c_style = doc.characterStyles;
};

//pallet
var plt = new Window('palette', "Dingbats Input Palette", undefined);
plt.margins = 5;

//group for buttons
var grp1 = plt.add('group');
grp1.orientation = 'column';
grp1.alignChildren = 'left';
grp1.spacing = 0;

//group for output
var grp2 = plt.add('group');

//Curry
Function.prototype.curry = function(){
  var slice = Array.prototype.slice, args = slice.apply(arguments) , that = this;
    return function (){
      return that.apply(this, args.concat (slice.apply(arguments)));
    };
};
var each_button_event = function (i) {
  insert_this( value_arr[i] );//click button input character with style
};

//create button matrix
var appver = app.version.split('.')[0];
var button_arr = [];
for (var i=0, iL=value_arr.length; i < iL ; i++) {
  var btn_row = appver === '5' ? 5 : 8; //cs3 5rows, cs4 8rows
  //switch rows count
  if (i%btn_row === 0){
    var bt_grp = grp1.add('group');
    bt_grp.spacing = 0;
  }
  button_arr.push(bt_grp.add('button', [0,0,23,23], value_arr[i]));  
  button_arr[i].onClick = each_button_event.curry(i);
};

//output text with style
var style_ddl = grp2.add('dropdownlist', [0,0,160,22], c_style.everyItem().name);
style_ddl.selection = 0;

//change character style, change applied font on button
style_ddl.onChange = function () {
  //set script ui font to buttons
  var out_family = c_style[style_ddl.selection.index].appliedFont;
  var out_style = c_style[style_ddl.selection.index].fontStyle == NothingEnum.NOTHING ? "" : c_style[style_ddl.selection.index].fontStyle;
  var set_a = ScriptUI.newFont(out_family, out_style, button_pointsize);//family, style, size
  set_font(grp1, set_a);
}

plt.show();

function insert_this (string) {
  if (app.selection.length === 1 && app.selection[0].hasOwnProperty('baseline')) {
    var tar = app.selection[0].insertionPoints;
    tar[-1].appliedCharacterStyle = c_style[style_ddl.selection.index];
    tar[-1].contents = string.charAt(0);//"&&" measure
  };
}

//by Peter Kahrel
function set_font (control, font) {
  for (var i=0, iL=control.children.length; i < iL ; i++) {
    if ("GroupPanel".indexOf(control.children[i].constructor.name) > -1){
      set_font(control.children[i], font);
    }
    else{
      control.children[i].graphics.font = font;
    }
  };
}
```