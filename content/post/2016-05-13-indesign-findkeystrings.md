+++
title = "indesign.jsx ロケールに依存しない名称取得"
date = "2016-05-13T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++



日本語化されている値の [段落スタイルなし]、[基本段落] みたいのをitemByNameしたいけど
ロケールに依存したくない時の名称の取得について。

`findkeyStrings` をつかう。

文字スタイルの [なし] の場合

```
$.writeln(app.findKeyStrings("\[なし\]"));

// => $ID/[No Character Style],$ID/#ConditionSetNone,$ID/[CopyFit None],$ID/FS no char style,$ID/#DIALOG_None,$ID/TV [none],$ID/[No character style],$ID/kSpecialNone,$ID/Not Specified,$ID/No hyperlink anchor points defined,$ID/NoneMaster,$ID/[None],$ID/No character style,$ID/NoDest,$ID/#COLOR_None,$ID/XML_None,$ID/#ConditionTag_DeleteNoReplaceChoice,$ID/kNoPreflightProfile,$ID/k[None]
```

文字スタイル以外で使用されている [なし] を含めた配列がかえるのでそれっぽいのを選択

```js
$.writeln(app.characterStyles.item("$ID/[No Character Style]").name);

// => [なし]
```

---

2016-05-20追記 戻すにはこれでいい

```js
$.writeln(app.translateKeyString("$ID/[No Character Style]"));

// => [なし]
```
