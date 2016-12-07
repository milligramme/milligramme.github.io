+++
title = "cliでディスク作成フォルダをつくる"
date = "2013-05-29T00:00:00+09:00"
tags = ["osx", "cli"]
+++



その昔、OSXのコンテキストメニューで「ディスク作成フォルダ」って作れたと思ったのに、今 (OSX 10.8.5) 見たらなくて
Disk Utilityからやるのがすごく不便なので、コマンドラインから出来ないか調べた

```
$ mkdir foo.fpbf
$ mv foo foo.fpbf
```

これでできる。同名フォルダとかがなければ一応可逆的に使える。

Finder でリネームしてもだめ

![2013 05 29 Cant Rename Fpbf](/images/2013-05-29_cant_rename_fpbf.png)