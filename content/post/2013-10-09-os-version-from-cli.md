+++
title = "cliでOSXのバージョンを確認"
date = "2013-10-09T00:00:00+09:00"
tags = ["osx", "cli"]
+++

`sw_vers` コマンドでいい

```
$ sw_vers
ProductName:  Mac OS X
ProductVersion:  10.7.5
BuildVersion:  11G63

$ sw_vers -productVersion
10.7.5
```
