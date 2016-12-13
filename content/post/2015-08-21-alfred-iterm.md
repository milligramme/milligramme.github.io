+++
title = "Alfred v2.7.2でiTermが使えるようにする"
date = "2015-08-21T00:00:00+09:00"
tags = ["alfred", "iterm", "applescript"]

outdated = true
+++

Alfred v2.7.2 の仕様変更でTerminalのかわりにiTermを使用していた機能が効かなくなったので修正した。

Terminal以外では

```
Alfred Preferences > General > Terminal / Shell
```

で

Application: 「Custom」 を選択して、その下のAppleScriptのフィールドに

[stuartcryan/custom\-iterm\-applescripts\-for\-alfred](https://github.com/stuartcryan/custom-iterm-applescripts-for-alfred)

のapplescriptをiTermのバージョンに合わせて、ペーストしてあげたらいい。

{{% img src="images/2015-08-21-alfred-iterm-integration.png" %}}

