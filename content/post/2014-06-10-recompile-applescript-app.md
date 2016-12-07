+++
title = "PPCなAppleScriptアプレットを再コンパイルする"
date = "2014-06-10T00:00:00+09:00"
tags = ["cli", "applescript"]
+++

![2014 06 10 No Support Ppc](/images/2014-06-10-no-support-ppc.png)

古いアドビのアプリなどについてるAppleScriptアプレットがPPC用になってて使えなくなってるのを直す

たとえば

```
/Applications/Adobe Illustrator CS5/Scripting.localized/Sample Scripts.localized/AppleScript.localized/Calendar.localized/Make Calendar.app
└── Contents
    ├── Info.plist
    ├── MacOS
    │   └── applet
    ├── PkgInfo
    └── Resources
        ├── InfoPlist.strings
        ├── Scripts
        │   └── main.scpt -----> これ
        ├── applet.icns
        ├── applet.rsrc
        └── description.rtfd
            └── TXT.rtf
```

「パッケージの内容を表示」して、 `Contents/Resources/Scripts/main.scpt ` をAppleScriptエディタ.appで開いて、改めてアプリケーション形式で書出しをすればいい

コマンドラインでもできる

```
$ osacompile [ソース] -o [書出し先]
```

くわしくは man

```bash
$ cd /Applications/Adobe\ Illustrator\ CS5/Scripting.localized/Sample Scripts.localized/AppleScript.localized/Calendar.localized
$ osacompile Make\ Calendar.app/Contents/Resources/Scripts/main.scpt -o _Make\ Calendar.app
```

コマンドラインだと同名上書きができないぽい

![2014 06 10 Ppc Applet](/images/2014-06-10-ppc-applet.png)
