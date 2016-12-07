+++
title = "new MacBook Air setup Pt.4"
date = "2013-10-23T00:00:00+09:00"
tags = ["osx", "defaults", "cli"]
+++

今日やった defaults, chflags コマンドメモ

### ウィンドウの端にきたときのぼよんぼよんを無効にする

```bash
$ defaults write -g NSScrollViewRubberbanding -int 0 
```

### ファイル不可視にする

```bash
$ sudo chflags hidden /PATH/TO/TARGET
```

### ファイルのアンロックする

```bash
$ sudo chflags -R nouchg /PATH/TO/TARGET
```

### 参考にした

- [Remove Scroll Bouncing in Lion: Apple Support Communities](https://discussions.apple.com/thread/3221799?start=15&tstart=0)
- [osx - Command to unlock "Locked" files on OS X - Super User](http://superuser.com/questions/40749/command-to-unlock-locked-files-on-os-x)
- [10.8.5 Supplemental makes "mach_kernel" visible - FineTunedMac](http://www.finetunedmac.com/forums/ubbthreads.php?ubb=showflat&Number=26982)