+++
title = "MavericksでAppleScriptのclickが効かない"
date = "2015-07-31T00:00:00+09:00"
tags = ["osx", "applescript"]
+++

AlfredのWorkflowに登録しているAppleScriptが正常に動作してなかったので調べた


```applescript
tell application "TextMate"
	activate
end tell

tell application "Mail"
	activate
end tell

tell application "LimeChat"
	activate
end tell

tell application "MenuMate"
	activate
	
	-- メニューにアイコンがある状態だと待機状態で止まってしまうので隠す
	-- click anywhere to hide startup menu
	tell application "System Events"
		tell process "Finder"
			click at {0, 0} -- <------ここでエラー
		end tell
	end tell
end tell
```


click at のところで "Can’t make {0, 0} into type list." とエラーになる。

根本的に解決策がみつからないので、do shell script + cliclick に置き換えた。

```applescript
tell application "System Events"
	do shell script "/usr/local/bin/cliclick 'c:0, 0'"
end tell
```

### 参考にした
- [applescript click at not working \| Apple Support Communities](https://discussions.apple.com/message/24712873)
- [AS Hole（AppleScriptの穴） By Piyomaru Software » MavericksのGUI Scriptingのclick命令にバグ » Blog Archive](http://piyocast.com/as/archives/2732)
- [OS X：Mavericks でアクセシビリティとセキュリティ機能を使った AppleScript の使用方法 \- Apple サポート](https://support.apple.com/ja-jp/HT202802)
