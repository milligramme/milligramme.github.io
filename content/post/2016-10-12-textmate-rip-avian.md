+++
title = "TextMate2のAvianフォルダの終了"
date = "2016-10-12T18:14:00+09:00"
tags = ["textmate"]

outdated = false 
+++

TextMate v2.0-beta.12.23 でバンドルなどを保存しておくフォルダが TextMateフォルダにまとまるようになった。

アップデート時にマイグレードしてくれる

![2016 10 12 Avian Migration](/images/2016-10-12-avian-migration.png)

```
~/Library/Application Support/TextMate  % tree -L 2
.
├── Bundles
│     ├── AppleScript.tmbundle
│     ├── CSS3.tmbundle
│     ├── HAML-Handcrafted.tmbundle
│     ├── HTML5.tmbundle
│     ├── Jsx.tmbundle
│     ├── Node.js.tmbundle
│     ├── PlantUML.tmbundle
│     ├── Processing.tmbundle
│     ├── RailsCasts.tmbundle
│     ├── Ruby-Slim.tmbundle
│     ├── Themes.tmbundle
│     ├── Whitespace.tmbundle
│     ├── XML.tmbundle
│     ├── rubocop.tmbundle
│     └── text.tmbundle
├── ClipboardHistory.db
├── ClipboardHistory.db-shm
├── ClipboardHistory.db-wal
├── Global.tmProperties
├── Managed
│     ├── Bundles
│     ├── Cache
│     └── LocalIndex.plist
├── RecentProjects.db
├── Ruby
│     └── 1.8.7
└── Session

~/Library/Application Support/Avian  % tree -L 2
.
└── Disable
```