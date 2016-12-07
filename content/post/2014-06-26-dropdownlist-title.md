+++
title = "ScriptUIのDropDownListでタイトルを一緒に設定できない"
date = "2014-06-26T00:00:00+09:00"
tags = ["scriptui", "extendscript"]
+++

ScriptUIの記述で "JavaScript Tools Guide CS5.pdf" P102 のサンプルコードに変な書き方があった

```js
var list = "Lorem ipsum dolor sit amet, consectetur adipisicing elit".split(" ");
w.ddl1 = w.add("dropdownlist { title: 'Image format:' }", u, list);
w.ddl2 = w.add("dropdownlist { title: 'Background color:' }", u, list); 
w.ddl3 = w.add("dropdownlist { title: 'Text color:' }", u, list); 
w.ddl1.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' }; 
w.ddl2.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' }; 
w.ddl3.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' };
```

add()するときに第1引数に scriptuiエレメントを指定するときに titleもいっしょに設定できるらしい

DropDownList以外に FlashPlayer, IconButton, Image, TabbedPanel にも使えるらしい

が、しかし

![2014 06 26 Dropdownlist Label](/images/2014-06-26_dropdownlist-label.png)

表示はされるもののドロップダウンリストにアクセスできない（クリックしても反応しない InDesign CS5にて）

というわけで、これなら弄れるようになる書き方

```js
var u;
var w = new Window('dialog', "dropdownlist-label", u);
w.orientation = 'column';
w.margins = 5;
w.spacing = 5;
w.alignChildren = ['fill', 'fill'];

var list = "Lorem ipsum dolor sit amet, consectetur adipisicing elit".split(" ");
w.ddl1 = w.add("dropdownlist", u, list);
w.ddl2 = w.add("dropdownlist", u, list);
w.ddl3 = w.add("dropdownlist", u, list);

w.ddl1.title = 'Image format:';
w.ddl2.title = 'Background color:';
w.ddl3.title = 'Text color:';
w.ddl1.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' }; 
w.ddl2.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' }; 
w.ddl3.titleLayout = { alignment: ['left', 'center'], spacing: 3, characters: 16, justify: 'right' };

w.show();
```

これだからAdobeは信用できない