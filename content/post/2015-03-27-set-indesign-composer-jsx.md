+++
title = "InDesignのコンポーザー変更"
date = "2015-03-27T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

[https://forums.adobe.com/message/7359998#7359998](https://forums.adobe.com/message/7359998#7359998)

InDesignの段落コンポーザーは通常、その言語の文字列で指定する。

```js
ParagraphStyle.composer = "Adobe 日本語単数行コンポーザー";
```

keystringで指定するとロケールに依存しないでコンポーザーを指定できる、たとえば

```js
ParagraphStyle.composer = "$ID/HL Single J";
```

でドイツ語版のInDesignでも日本語版の単数行コンポーザーの**指定**ができるようになる。

keystring は app.findKeyStrings("メニュー項目") で配列で取得できる。

```js
var doc = app.documents.add();  
var pstyle = doc.paragraphStyles.add();

// デフォルトのコンポーザ
$.writeln(pstyle.composer); // => Adobe 日本語段落コンポーザー  

pstyle.composer = "Adobe 日本語単数行コンポーザー";  
$.writeln(pstyle.composer); // => Adobe 日本語単数行コンポーザー 


// Adobe 多言語対応単数行コンポーザー
// これはロケールの違う文字列だからダメ
try {
  pstyle.composer = "Adobe World-Ready Single-line Composer";
  $.writeln(pstyle.composer);
}
catch(x_x){
  $.writeln([x_x.message,x_x.line,$.stack,].join("\n"));
}

try {
  pstyle.composer = "Adobe 多言語対応単数行コンポーザー";
  $.writeln(pstyle.composer);
}
catch(x_x){
  $.writeln([x_x.message,x_x.line,$.stack,].join("\n"));
}

app.findKeyStrings("Adobe 日本語単数行コンポーザー"); //=> ["$ID/HL Single J"]
app.findKeyStrings("Adobe 日本語段落コンポーザー "); //=> ["$ID/HL Composer J"]
app.findKeyStrings("Adobe 多言語対応単数行コンポーザー"); //=> ["$ID/HL Single Optyca"]

var s = app.findKeyStrings("Adobe 多言語対応段落コンポーザー");
$.writeln(s.toSource()); // ["$ID/HL Composer Optyca"]

pstyle.composer = s[0];
$.writeln(pstyle.composer); // => Adobe 多言語対応段落コンポーザー
```