+++
title = "ダイアログ内で $.localize を有効にする"
date = "2011-05-18T00:00:00+09:00"
tags = ["extendscript", "scriptui"]
+++

InDesignで、$.localize = true; にして多言語機能を有効にした状態で
組込ダイアログ、ScriptUIダイアログ内で 表示されなくてちょっと悩んだのでメモ。

通常は、多言語オブジェクトをつくれば、現在のLocaleでアラートを表示します。

```js

$.localize = true;

var err_obj = {
  ru:"Я не могу съесть омлет!",
  en:"I can't eat the omelette.",
  ja:"私はオムライスを食べられません"
  };

// alert
alert($.locale + ": " + err_obj);
```

![/images/2011/05/localize_alert.png](/images/2011/05/localize_alert.png)

しかしダイアログ内では、$.localize = true にしててもオブジェクトを評価してくれないのか

そのままでは表示されません。

### 組込ダイアログ

![/images/2011/05/localize_builtin_dlg.png](/images/2011/05/localize_builtin_dlg.png)

### ScriptUIダイアログ

![/images/2011/05/localize_scriptui_dlg.png](/images/2011/05/localize_scriptui_dlg.png)

いろいろ試した結果

- Object.toString()で文字列にしてみる => イケタ
- Object+"" で文字列にしてみる => イケタ
- Object[$.locale.substr(0,2)]で値を取り出す => イケタ（めんどう）

単に、文字列にすれば大丈夫みたい。

```js
// InDesign Builtin Dialog
var dlg_bl = app.dialogs.add({name:"Builtin Dialog　♥　$localize"});
with (dlg_bl.dialogColumns.add()){

  with (dialogRows.add().borderPanels.add()){
    with (dialogColumns.add()){
      staticTexts.add({staticLabel:$.locale});
      staticTexts.add({staticLabel:err_obj}); // 表示されない [Object object]
      staticTexts.add({staticLabel:err_obj.toString()});
      staticTexts.add({staticLabel:err_obj + ""});
      staticTexts.add({staticLabel:err_obj[$.locale.substr(0,2)]});
    }
  }
}
var flg = dlg_bl.show();
if (!flg) {
  dlg_bl.destroy();
  exit();
}

// ScriptUI Dialog
var dlg_su = new Window('dialog', "ScriptUI　♥　$localize");
var p = dlg_su.add('panel');
p.grp1 = p.add('group');
p.grp1.orientation = 'column'
p.grp1.add('statictext', undefined, $.locale);
p.grp1.add('statictext', undefined, err_obj); // 表示されない [Object object]
p.grp1.add('statictext', undefined, err_obj.toString());
p.grp1.add('statictext', undefined, err_obj + "");
p.grp1.add('statictext', undefined, err_obj[$.locale.substr(0,2)]);

dlg_su.show();
```