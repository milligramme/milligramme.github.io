+++
title = "deploy-gitbucket-on-pm2"
date = "2013-12-12T00:00:00+09:00"
tags = ["gitbucket"]
+++

ローカルwikiと化してる [takezoe/gitbucket](https://github.com/takezoe/gitbucket) を [Unitech/pm2](https://github.com/Unitech/pm2) 上で動かしてみる

用意するもの

- pm2
- gitbucket.war
- 起動用スクリプト

```
npm install -g pm2
```

pm2のドキュメントにいろいろ書いてある

起動用のスクリプトを用意 gitbucket.sh とか

```bash
#!/usr/bin/env bash

java -jar /PATH/TO/gitbucket.war --port=8081 --prefix=/gitbucket
```

pm2 上に gitbucket (http://localhost:8081/gitbucket) が立ち上げる、  `pm2 list` で確認

```
$ pm2 start gitbucket.sh
$ pm2 list
```

止め方

```
$ pm2 stop gitbucket.sh
$ pm2 delete gitbucket.sh
$ pm2 kill
```

で（プロセス一時停止、プロセス削除、pm2の終了）できる。ただし、gitbucketの場合は、pm2をkillしても jetty のプロセスが活きたままなのだけど、こういうものとして運用してみる(ふつうに kill -9 PID する)


多重起動用とか引数が必要な時は、jsonで指定してあげるといい

```json
[{
  "name":"gitbucket",
  "script":"/PATH/TO/gitbucket.sh",
  "args":"--port=8081 --gitbucket.home=/PATH/TO/GB_HOME"
}, {
    "name":"another_app",
    "script":"/PATH/TO/another.js"
}]
```

オプションでクラスタリングとか、cronオプションでの再起動とかも設定できる