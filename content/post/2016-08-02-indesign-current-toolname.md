+++
title = "indesign.jsxで現在の選択ツールを返す"
date = "2016-08-02T10:25:00+09:00"
tags = ["indesign", "extendscript"]
+++

前々からあるけど indesign.jsxで現在の選択ツールを返す方法が出てたのでメモ

[Is it possible to get selected tool name from a\.\.\. \|Adobe Community](https://forums.adobe.com/thread/2189391)

```js
//@target "InDesign"
$.writeln(app.toolBoxTools.currentTool);
$.writeln(app.toolBoxTools.currentToolName);
$.writeln(app.findKeyStrings(app.toolBoxTools.currentToolName)); // localeに依存しない

// TYPE_TOOL
// 横組み文字ツール
// $ID/Type Tool

$.writeln(app.toolBoxTools.properties.toSource());

// ({currentToolName:"横組み文字ツール", currentToolHint:"<i>テキストフレームを作成するか、テキストを選択します。</i><br /><br /><b>修飾キー :</b><br /><b>Shift :  </b>新規テキストフレームの縦横比を固定します。<br /><b>Command :  </b>一時的に選択ツールに切り替えます。<br /><b>Option :  </b>テキストの編集時に手のひらツールに切り替えます。テキストフレームの作成中は、中心から外側に向かって描画します。<br /> <br />単語を選択するにはテキストをダブルクリックします。行を選択するには 3 回クリックします。段落を選択するには 4 回クリックします。ストーリーを選択するには 5 回クリックします。<br /><br /><b>ショートカット :</b> T", currentToolIconFile:new File ("/Applications/Adobe%20InDesign%20CC/Adobe%20InDesign%20CC.app/Contents/MacOS/Required/Text%20Editor.InDesignPlugin/Versions/A/Resources/idrc_PNGA/1000.idrc"), currentTool:({}), parent:resolve("/")})

```

`app.toolBoxTools.currentTool` が文字列で返ってきてるけど

```js
UITools.TYPE_TOOL == 1954107508
```

のこと

currentToolHintは htmlで入ってる。


[InDesign以外だとちょっと違うみたい](https://forums.adobe.com/thread/2189391#8913946)