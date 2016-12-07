+++
title = "dateのフォーマット"
date = "2014-01-08T00:00:00+09:00"
tags = ["cli", "bash"]
+++

shellscriptでログファイルにフォーマット付の日付をつけたい

```bash
$ date
2014年 1月 8日 水曜日 10時31分23秒 JST
```

date '+FORMAT' とすればいい

時間が特に不要なら '+%F' でよさそう

```bash
$ date '+backup-%Y%m%d-%H%M.log'
backup-20140108-1032.log

$ date '+backup-%F.log'
backup-2013-01-18.log
```

### 参考にした

[日付を取得する - UNIX & Linux コマンド・シェルスクリプト リファレンス](http://shellscript.sunone.me/date.html#date-3)