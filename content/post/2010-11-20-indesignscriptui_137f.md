+++
title = "文字コードをずらすScriptUIパレット"
date = "2010-11-20T00:00:00+09:00"
tags = ["indesign", "scriptui", "extendscript"]
+++
必要に迫られてScriptUIパレットを作ることになったので、ウォーミングアップ（=理解）のために、以前書いた文字コードをずらすスクリプトを実装してみました。ダミーテキストをつくったりくらいには使えるかもしれません。

パレットなので、InDesignアプリのスクリプトパネルから実行してください。（ESTKなどから実行するのとアプリから実行するのの判定ってあったような気がするんだが…）

テキスト選択するとスライダーとテキストプレビューがパレットで現れます。閉じるときは左上のボタンで。

![](/images/2010/11/charcode_palette2.png)

スライダーを動かすとプレビューの文字コードがその分ズレます。

![](/images/2010/11/charcode_palette3.png)

applyボタンを押すと本文に適用します。プレビューされててもフォントが持っていない文字は化けます。undoボタンでやり直せます。

![](/images/2010/11/charcode_palette4.png)

reselectボタンを押すと現在の選択状態を取得してプレビューを更新してスライダーをゼロに戻します。

![](/images/2010/11/charcode_palette5.png)

選択範囲をかえたら一度reselectボタンを押して更新（それまでは前回のテキスト選択を保持）

![](/images/2010/11/charcode_palette6.png)

で、スライダー動かし、applyボタン。スライダーのレンジは±50ですが、apply, reselectの繰り返しでそれ以上の範囲にズラせます。

![](/images/2010/11/charcode_palette7.png)

再選択したときにテキスト以外のオブジェクト（テキストフレーム、表組み、グラフィックなど）だとアラートがでます。

![](/images/2010/11/charcode_palette8.png)

その他

スライダーを動かして、文字コードが0x0020（スペース）より小さくなる場合はズラす処理をしません（制御文字になっちゃうから）

```js
/**
 * charcode shift palette
 */
#targetengine "session"
// documents open?
if (app.documents.length === 0 || app.selection.length === 0) {
  exit();
};
if (app.selection.length !== 1) {
  exit();
};

var selObj = app.selection[0];
// text object?
if (selObj.hasOwnProperty('baseline') == false) {
  exit();
}
var selc = selObj.contents;
var plt = new Window('palette', "Char. Code Shifter", undefined);
plt.panel = plt.add('panel');
// add elements
var slider_label = plt.panel.add('statictext',[0,0,40,22], "0")
var codeshift_slider = plt.panel.add('slider',[0,0,190,22], 0, -50, 50);
var content_text = plt.panel.add('statictext', [0,0,190,150], selc, {multiline: true});
// initialize
var pitch = 0;
var mode = 0;
// add buttons with grouping
plt.btn_grp = plt.add('group');
var codeshift_btn = plt.btn_grp.add('button', [0,0,69,23], "apply");
var resel_btn = plt.btn_grp.add('button', [0,0,69,23], "reselect");
var undo_btn = plt.btn_grp.add('button', [0,0,46,23], "undo");

//callbacks
//slider
codeshift_slider.onChanging = function () {
  var v = Math.round(this.value);
  slider_label.text =  v;
  pitch = v;
  mode = 1;
  char_code_shift(selc);
}
//character code shift
codeshift_btn.onClick = function () {
  pitch = slider_label.text * 1.0;
  mode = 2;
  char_code_shift(selc);
}
//get selection again
resel_btn.onClick = function () {
  codeshift_slider.value = pitch =  0;
  slider_label.text = 0;
  if (app.selection[0].hasOwnProperty('baseline')) {
    selc = app.selection[0].contents;
  };
  mode = 1;
  char_code_shift(selc);
}
//undo
undo_btn.onClick = function () {
  app.documents[0].undo();
}
plt.show();

/**
 *  char code shift
 *
 * if text object contents shift character code with slider's value.
 * but dont convert to the character code from 0x00 to 0x1f.
 *
 * when re-select other than text , return alert
 *
 * @param {String} selc Selection contents
 */
function char_code_shift (selc) {
  if (app.selection[0].hasOwnProperty('baseline') && mode !== 0) {
    var replace_code_shift = "";
    for (var si=0, siL=selc.length; si < siL ; si++) {
      if (selc[si].charCodeAt(0) !== 'd') {
        var ff = (selc[si].charCodeAt(0) + pitch) > 32 //'space' = 33
          ? (selc[si].charCodeAt(0) + pitch).toString(16) //convert if forward of 'space'
          : selc[si].charCodeAt(0).toString(16); //dont convert if behind of 'space'
        replace_code_shift += String.fromCharCode('0x'+ff);
      }else{
        replace_code_shift += selc[si];
      }
    };
    if (mode === 1) {
      content_text.text = replace_code_shift;
    };
    if (mode ===2) {
      app.selection[0].contents = replace_code_shift;
    };
  }
  else{
    alert('Selection Error');
  }
}

```