+++
title = "$.write（）と$.writeln（）とESTKとごめんなさい"
date = "2010-07-15T00:00:00+09:00"
tags = ["extendscript", "illustrator", "indesign", "photoshop"]
+++
ごめんなさい、懺悔します。

以前から一部の自前のスクリプトで何故か ExtendScript Toolkit が勝手に立ち上がるときがあるなと思っていたのですが、最近はほとんど ESTK を使わない日々が続き、今日やっと理由がわかりました。

（今は [TextMate](http://macromates.com/) 使い） 

ExtendScript のスクリプトの中に `$.writeln( )` や `$.write( )` がある（コメントアウトされていない）と InDesignのScriptPanelなどのアプリケーションから実行しても、裏で ExtendScript Toolkit が勝手に起動してしまう（CS3だとESTK2がDOMのXMLをちくちく読み込みに行き、そしてXML読み込みに失敗する）ことになるのを知りました。

![/images/2010/09/772-estk_importing_xml.png](/images/2010/09/772-estk_importing_xml.png)

`$.writeln( )`, `$.write( )` はちゃんとコメントアウトします。反省。

```js
//ESTKを立ち上げていない状態で
//アプリケーションから実行すると、ESTKが立ち上がってくれます。

$.writeln("Hello ESTK from backyard");
```