+++
title = "gitbucketをhomebrewでインストール"
date = "2014-04-02T00:00:00+09:00"
tags = ["gitbucket", "homebrew"]
+++

gitbucketがhomebrewで入るようになってたので確認

```
% brew install gitbucket
==> Downloading https://github.com/takezoe/gitbucket/releases/download/1.10/gitbucket.war
######################################################################## 100.0%
==> Caveats
Note: When using launchctl the port will be 8080.

To have launchd start gitbucket at login:
    ln -sfv /usr/local/opt/gitbucket/*.plist ~/Library/LaunchAgents
Then to load gitbucket now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.gitbucket.plist
Or, if you don't want/need launchctl, you can just run:
    java -jar /usr/local/opt/gitbucket/libexec/gitbucket.war
==> Summary
🍺  /usr/local/Cellar/gitbucket/1.10: 3 files, 42M, built in 17 seconds
```

デフォルトでポートが 8080 になってるので `launchctl unload ~~` して

`/usr/local/opt/gitbucket/homebrew.mxcl.gitbucket.plist` を修正 `launchctl load ~~` し直す

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>gitbucket</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/bin/java</string>
      <string>-Dmail.smtp.starttls.enable=true</string>
      <string>-jar</string>
      <string>/usr/local/opt/gitbucket/libexec/gitbucket.war</string>
      <string>--host=127.0.0.1</string>
      <string>--port=8081</string>
      <string>--https=true</string>
    </array>
    <key>RunAtLoad</key>
   <true/>
  </dict>
</plist>
```

2014-04-02現在、バージョン最新版は 1.12 だけど 1.10が入る
