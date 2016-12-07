+++
title = "ScriptPanel内の不可視ファイルを消したい"
date = "2011-04-15T00:00:00+09:00"
tags = ["indesign"]
+++
InDesignのスクリプトの設定を記憶するファイルとしてテキストファイルを不可視のフォルダに入れて管理しようという試みで、保存場所をスクリプトと同じ階層にしていると、設定ファイルが ScriptPanelに表示されてしまってかっこ悪いので隠したい。

![/images/2011/04/show_unsupported.png](/images/2011/04/show_unsupported.png)

ということで調べていたら、スクリプトパネルのメニュー「サポートされていないファイルとを表示」のチェック外すと、
![/images/2011/04/hide.png](/images/2011/04/hide.png)

とりあえずテキストファイルは表示されなくなりました（.gitignoreなども消えました）。不可視ファイルというわけでないので、相変わらず不可視フォルダは見えたまま

![/images/2011/04/hide_unsupported.png](/images/2011/04/hide_unsupported.png)

.js .jsx .jsxbin .scpt .applescript など以外のファイルは表示されなくなる。不可視のjsファイルは表示されます（Finderでは表示されない）。なぜか .jsxinc （インクルードファイル）はサポートされていない

**結論**
スクリプトパネルで不可視ファイルの.(dot)付フォルダやファイルを表示しない方法というのはいまのところ見つかってないが、テキストファイルなどのグレーアウトしてるものなら隠せる。

**おまけ**
サポートされていないファイルとを表示」のチェック外すスクリプト。

```js
var m = app.menuActions.item('$ID/DisplayUnsupportedFiles');
if (m.checked == true) {
  m.invoke();
};
```
