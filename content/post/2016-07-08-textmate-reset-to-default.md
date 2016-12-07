+++
title = "TextMate2の設定をリセット"
date = "2016-07-08T10:25:00+09:00"
tags = ["textmate"]
+++

TextMate2 (2.0-beta.11.5) で検索ダイアログのコピペが効かなくなった
（再描画がされなくなった）ので、リセットしてみた。

[Reverting To Defaults · textmate/textmate Wiki](https://github.com/textmate/textmate/wiki/Reverting-To-Defaults)

これのうち 4. だけ行って解消された。

```
% ls ~/Library/Preferences/com.macromates*
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.LSSharedFileList.plist
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.LSSharedFileList.plist.TNPsjYu
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist.17xUhLC
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist.1Srm9ZW
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist.2SXluH1
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist.O3MMWUQ
/Users/gdansk/Library/Preferences/com.macromates.TextMate.preview.plist.owfhhHQ
/Users/gdansk/Library/Preferences/com.macromates.textmate.paste_online.yaml
/Users/gdansk/Library/Preferences/com.macromates.textmate.webpreview.plist
```

これらをバックアップ取ってから削除。