+++
title = "共通点を探す旅（おまじない編）"
date = "2009-08-05T00:00:00+09:00"
tags = ["extendscript"]
+++
JavaScript（ExtendScript）でアラートをスキップしてしまいたいようなとき。

または、ダイアログがでるようにリセット（おまじない）したいときInDesign, Illustrator, Photoshopで違っているのでメモ。

### #target 'indesign'

```js
//ダイアログもアラートも出す
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALL;
//ダイアログもアラートも出さない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.NEVER_INTERACT;
//ダイアログさない、アラートは出す。
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.INTERACT_WITH_ALERTS;
```

### #target 'illustrator'

```js
//ダイアログ出さない
app.userInteractionLevel = UserInteractionLevel.DONTDISPLAYALERTS;
//ダイアログ出す
app.userInteractionLevel = UserInteractionLevel.DISPLAYALERTS;
```

### #target 'photoshop'

```js
//ダイアログもアラートも出す
app.displayDialog = DialogModes.ALL;
//ダイアログもアラートも出さない
app.displayDialog = DialogModes.NO;
//ダイアログさない、アラートは出す。
app.displayDialog = DialogModes.ERROR;
```

なぜかIllustratorだけ、2種類しかない。