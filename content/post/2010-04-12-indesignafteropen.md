+++
title = "ドキュメントを開くとき（afterOpenイベント）に自分仕様の設定で開くスクリプト"
date = "2010-04-12T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignで「ドキュメントを開く」イベントのときに

毎回、フレーム枠を表示させたとか、制御文字を表示させたいなど、〜〜する時の豆スクリプト。

厳密には、開くという命令が出て、ドキュメントを開いた後にスクリプトを自動的に実行させます。

[http://twitter.com/buruge/status/12018472868](http://twitter.com/buruge/status/12018472868)

> "あぁ...なんで開くファイルことごとくフレーム枠が非表示なのだ...。社内だと表示派は圧倒的少数なのかな...作業こわいわー。 "

をみて、EventListener使えばできると思いつつ、イベントまわりの経験値低めなので試してみました。

CS3以降で動作すると思います。

ScriptPanel（/Users/xxxx/Library/Preferences/Adobe InDesign/Version 5.0-J/Scripts/Scripts Panel/）にいれてInDesignから実行しておくと、
それ以降（InDesignを終了するまで）ガイド、フレーム枠、ルーラー、制御文字を表示した状態でドキュメントを開きます。

```js
#targetengine "session"
main();
function main(){
  var afOp = app.addEventListener("afterOpen", showGuidesAndFrame, false);
}
function showGuidesAndFrame (myEvent){
  var doc = myEvent.parent;
  with(doc){
    guidePreferences.guidesShown=true; //ガイド表示
    viewPreferences.showFrameEdges = true; //フレーム枠表示
    viewPreferences.showRulers = true; //ルーラー表示
    textPreferences.showInvisibles = true; //制御文字表示
  }
}
```

EventListerはInDesignが終了するまでの間保持されます。
もし途中でやめるならScriptPanelからEventListerを削除するスクリプトを実行します。

```js
#targetengine "session"
app.removeEventListener("afterOpen", showGuidesAndFrame, false);
```


また、InDesignを起動するたびに、この「開くときにこうする」のスクリプトを実行する起動項目的使い方をするならStartup Scriptsフォルダに入れておきます。

（/Users/xxxx/Library/Preferences/Adobe InDesign/Version 5.0-J/Scripts/Startup Scripts/）

応用すればいろいろ使えますが、いろんな意味で自分仕様なるので自己責任でお願いします。