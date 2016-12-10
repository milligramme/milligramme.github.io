+++
date = "2016-12-10T01:06:30+09:00"
description = ""
tags = ["terminal-notifier","ruby"
]
draft = false
outdated = false
title = "terminal-notifierでurl openが効かない"

+++

alfredで terminal-notifier をつかったrubyスクリプトで、
クリックしてもurl openが効かなくなったので調べた

結論からいくと timeoutを設定( nilでもいい) すればいいらしい

```
% sw_vers
ProductName:	Mac OS X
ProductVersion:	10.10.5
BuildVersion:	14F2009

% ruby -v
ruby 2.3.2p217 (2016-11-15 revision 56796) [x86_64-darwin14]
```

[\`\-open\` does not work in 10\.11 · Issue \#181 · julienXX/terminal\-notifier](https://github.com/julienXX/terminal-notifier/issues/181)

```rb
require "terminal-notifier"

include TerminalNotifier

# url
url = "https://google.com/"

# 以前はこれでも機能した
notify "test1a", {title: "wont open url", open: url}
# timeoutを指定しないと機能しない
notify "test1b", {title: "open url", open: url, timeout: 2}

# openのかわりにexecute
notify "test2a", {title: "wont open url", execute: 'open "#{url}"'}
# timeoutを指定しないと機能しない
notify "test2b", {title: "open url", execute: "open \"#{url}\"", timeout: 2}
notify "test2c", {title: "open url", execute: "open \"#{url}\"", timeout: nil}

# webブラウザを指定
notify "test3a", {title: "open url", execute: "open \"#{url}\" -a "Google Chrome.app"", timeout:3}
notify "test3b", {title: "open url", execute: "open \"#{url}\" -a safari", timeout:3}
notify "test3c", {title: "open url", execute: "open \"#{url}\" -a firefox", timeout:3}

# logfile
logfile = "file://$HOME/Library/Logs/Adobe Illustrator 17/AIGeneral.log"

notify "test4a", {title: "wont open file", execute: "open \"#{logfile}\""}
notify "test4b", {title: "open file", execute: "open \"#{logfile}\"", timeout: 5}
```