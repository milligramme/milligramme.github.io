+++
title = "UserInteractionLevelのちがい"
date = "2010-10-08T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
スクリプトで自動処理をしている時、ドキュメントのリンク切れ、更新、フォントの有無などでいちいち止まらないように、

```js
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.NEVER_INTRACT
```

などを記述してダイアログなどの発生を抑制することができます。

それらをExetendScript Toolkitの「オブジェクトモデルビューア」でみてみると3種類の値についてこう書いてあります。

> **UserInteractionLevels.INTERACT_WITH_ALL**
>
> The script displays all dialogs and alerts.（すべてのダイアログとアラートを表示）
>
> **UserInteractionLevels.NEVER_INTRACT**
> The script does not display any dialogs or alerts.（ダイアログもアラートも表示しない）
>
> **UserInteractionLevels.INTERACT_WITH_ALERTS**
> Displays alerts but not dialogs.（アラートは表示するが、ダイアログは表示しない）

これらについて、ちょっと気になるところがあったので実験してみました。

### 実験
予め用意したリンク切れとフォント不足したドキュメント（"~/Desktop/temp/alert_test.indd"）を用意して、下記のスクリプトを実行。

**わかったこと**

  - アラート、プロンプト、確認ダイアログ、フォルダ選択はいつでも表示される
  - ダイアログとはInDesign組み込みダイアログのころ（NEVERとWITH_ALERTで表示されない）
  - scriptUIダイアログはNEVER_INTRACTでも表示される

**わからないこと**

  - INTERACT_WITH_ALERTがアラート（リンク切れ・フォント不足）を表示しないけどいいの？
  - そうなるとNEVER_INTRACTとINTERACT_WITH_ALERTSの差違って何


もしかしたらテストの仕方がたりないのかもしれません。MacOSX 10.6 + InDesign CS3, CS4で実験しました。

![/images/2010/10/dialogs_alerts.png](/images/2010/10/dialogs_alerts.png)

```js
/**
 *  user interaction level test
 *
 *  Created by mg on 2010-10-07.
 */

//with_all
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;
testPack ("all");
//アラート、プロンプト、確認ダイアログ、フォルダ選択、組み込みダイアログ、ScriptUIダイアログ出る
//エラーメッセージ出る

app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;

//never
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.NEVER_INTERACT;
testPack ("never");
//アラート、プロンプト、確認ダイアログ、フォルダ選択、ScriptUIダイアログ出る
//組み込みダイアログ出ない
//リンク切れ・フォント無しエラーメッセージ出ない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;

//with_alert
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALERTS;
testPack ("alert");
//アラート、プロンプト、確認ダイアログ、フォルダ選択、ScriptUIダイアログ出る
//組み込みダイアログ出ない
//リンク切れ・フォント無しエラーメッセージ出ない？？？？？？
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;

function testPack (string) {
  alert(string);
  prompt(string, string);
  confirm(string, true);
  Folder.selectDialog();

  //dialog
  var dlg = app.dialogs.add({name:string});
  dlg.show();
  dlg.destroy();

  //scriptUI dialog
  var dlg_scUI = new Window(
    "dialog {\
      okBtn: Button {text:'"+ string +"', properties:{name:'ok'} }\
      }");
  dlg_scUI.center();
  dlg_scUI.show();

  if (string === "never" || string === "alert") {
    //indd has invalid fonts and links
    app.open(File("~/Desktop/temp/alert_test.indd"));
    //skipped modal dialog
    $.sleep(1000);
    app.documents[0].close();
  };
}
```