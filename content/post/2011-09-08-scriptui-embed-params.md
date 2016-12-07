+++
title = "スクリプト自身にパラメータ埋め込み"
date = "2011-09-08T00:00:00+09:00"
tags = ["extendscript", "indesign", "scriptui"]
+++
ちょっと思いついたので試してみた、ScriptUIでスクリプトファイル本体にパラメータを書き込ませて挙動を変化させる実験。

1行目の mode の値で分岐します。デフォルトでは 0 なので alert で "A" が返ります。

![/images/2011/09/params.png](/images/2011/09/params.png)

ScriptUIダイアログで押したボタンによって、この mode の値を変えて追記モードで書き込みます。

B のボタンを押すと次回実行時には1行目は 

```
var mode = 1;
```

となっていて "B" が返ります。という実験。

書き込むパラメータは複数行でもOK、ただし空行十分に用意しておくこと。

あと、TextMate の [ExtendScript Bundle](https://github.com/kanemu/extendscript-tmbundle) や 

Python の  [ind.py](http://www.my-notebook.net/3a58080c-851a-4806-8547-b6d55ee0daf4.html)  など

の OSAScript経由で本体を複製して実行するような実行形式だと使えません。

また、ESTK (ExtendScript Toolkit)ではハングアップしてしまいました。

あまり使えませんね。

```js
var mode = 0;

// DON'T write/change anything above here //

// embed param itself test

if (mode === 0) {alert("A");}
else  {alert("B");}

var u;
var w = new Window('dialog', "params", u);
w.orientation = 'column';
w.alignChildren = ['fill', 'fill'];

var a = w.add('button', u, "A");
var b = w.add('button', u, "B");

a.onClick = function () {
  f = File(app.activeScript);
  f.encoding = "UTF-8";
  f.open('e');
  f.seek(0,0);
  f.writeln("var mode = 0;"); // rewrite params
  f.close();
  w.close();
}

b.onClick = function () {
  f = File(app.activeScript);
  f.encoding = "UTF-8";
  f.open('e');
  f.seek(0,0);
  f.writeln("var mode = 1;"); // rewrite params
  f.close();
  w.close();
}

w.show();
```