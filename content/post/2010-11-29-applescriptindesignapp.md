+++
title = "InDesignのスクリプトフォルダを開くapp"
date = "2010-11-29T00:00:00+09:00"
tags = ["applescript", "indesign"]

outdated = true
+++

普段いろいろなInDesignのバージョンのスクリプトのテストをすることが多いので、Finderのサイドバーに Scripts Panel フォルダや Scripts フォルダを作って、スクリプトやそのフォルダ、エイリアスなんかを出したり入れたりする。

一般的？にはInDesignを起動して〜、スクリプトパネルを開いて〜、サブメニューのFinderで表示して〜、スクリプトをコピー（CS3以降）とかアプリをコマンド+クリックして〜、Presetsフォルダを開いて〜、Scriptsフォルダにスクリプトをコピー（CS2以前）
などというようにしている人が見受けられたので、それぞれのバージョンのスクリプトフォルダをひらくAppleScriptを書いてみた。

ベタにそれぞれ書くと

### CS5用

```applescript
tell application "Finder"
  open folder "Scripts Panel" of folder "Scripts" of folder "ja_JP" of folder "Version 7.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  display dialog "Put the scripts or folder or these alias here."
end tell
```

### CS4用

```applescript
tell application "Finder"
  open folder "Scripts Panel" of folder "Scripts" of folder "ja_JP" of folder "Version 6.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  display dialog "Put the scripts or folder or these alias here."
end tell
```

### CS3用

```applescript
tell application "Finder"
  open folder "Scripts Panel" of folder "Scripts" of folder "Version 5.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  display dialog "Put the scripts or folder or these alias here."
end tell
```

### CS2用

```applescript
tell application "Finder"
    open folder "Scripts" of folder "Presets" of folder "Adobe InDesign CS2_J" of folder "Applications" of startup disk
  display dialog "Put the scripts or folder or these alias here."
end tell
```

### CS用

```applescript
tell application "Finder"
  open folder "Scripts" of folder "Presets" of folder "Adobe InDesign CS_J" of folder "Applications" of startup disk
  display dialog "Put the scripts or folder or these alias here."
end tell
```

でも、これじゃアレなので同じアプレットのファイル名を変えたら挙動が変わるのにしてみます。

同じアプレットを複製して一文字目の数字によって開くバージョンを変えるのにしてみます。1〜5以外の数字は無視します。

### Download Sample(CS4用)

<a href='/images/2010/11/4open_id_scripts_folder.app_.zip'>4open_id_scripts_folder.app.zip </a>

（リネームして、1vvv.app => cs用、2www.app => cs2用、3xxx.app => cs3用、4yyy.app => cs4用、5zzz.app => cs5用　になります）

```applescript
tell application "Finder"
  set appname to name of (path to me) as string
  set namename to characters of appname
  set ver to item 1 of namename as number

  if ver = 1 then
    set openpath to folder "Scripts" of folder "Presets" of folder "Adobe InDesign CS_J" of folder "Applications" of startup disk
  else if ver = 2 then
    set openpath to folder "Scripts" of folder "Presets" of folder "Adobe InDesign CS2_J" of folder "Applications" of startup disk
  else if ver = 3 then
    set openpath to folder "Scripts Panel" of folder "Scripts" of folder "Version 5.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  else if ver = 4 then
    set openpath to folder "Scripts Panel" of folder "Scripts" of folder "ja_JP" of folder "Version 6.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  else if ver = 5 then
    set openpath to folder "Scripts Panel" of folder "Scripts" of folder "ja_JP" of folder "Version 7.0-J" of folder "Adobe InDesign" of folder "Preferences" of folder "Library" of home
  else
    return
  end if
  open openpath
end tell
```

自分はSidebar派なのでこんな感じ

![/images/2010/11/sidebar.png](/images/2010/11/sidebar.png)