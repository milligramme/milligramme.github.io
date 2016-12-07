+++
title = "gititをlaunchdで自動起動"
date = "2012-12-27T00:01:00+09:00"
tags = ["osx"]

outdated = true
+++

メモり用のローカルwikiの [gitit](http://gitit.net/) を今まで手動で

```
$ cd /path/to/gitit_wiki_dir
$ gitit -f gitit.conf
```

と起動していたのですが、plistを用意して自動起動化してみることにしてみた。

下記のような net.johnmacfarlane.gitit.plist を自分の環境にあわせて編集、/Library/LaunchAgentsフォルダにコピーしてloadする（pathはフルパスで）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>WorkingDirectory</key>
  <string>/Users/gdansk/gwiki</string>
  <key>KeepAlive</key>
  <true/>
  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/usr/bin:/bin:/usr/sbin:/sbin:/Users/gdansk/Library/Haskell/bin</string>
  </dict>
  <key>ProgramArguments</key>
  <array>
    <string>gitit</string>
    <string>-f</string>
    <string>gitit.conf</string>
  </array>
  <key>Label</key>
  <string>net.johnmacfarlane.gitit</string>
</dict>
</plist>
```

```
$ launchctl load /Library/LaunchAgents/net.johnmacfarlane.gitit.plist
```

これで localhost:5001 にアクセスすればいつでもgititを使用できる

### 参考にした

- [http://koweycode.blogspot.jp/2010/12/personal-gitit-wiki-on-macos-x.html](http://koweycode.blogspot.jp/2010/12/personal-gitit-wiki-on-macos-x.html)
- [LaunchDaemons (launchctl, launchd.plist) の使い方](http://www.maruko2.com/mw/LaunchDaemons_(launchctl,_launchd.plist)_の使い方)