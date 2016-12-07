+++
title = "外部から接続できない railsアプリ"
date = "2015-07-03T00:00:00+09:00"
tags = ["rails"]
+++

railsアプリ、middlemanアプリを
LAN内の別のOSX環境のブラウザから確認するのに、`http://固定IP:ポート` で railsアプリだけがアクセスできなかった

まず教えてもらった lsof でポートが使われているかの確認。

```
% sudo lsof -i :3000
```

```
% be rails s -p 3000

% sudo lsof -i :3000
COMMAND  PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
ruby    6680 gdansk   12u  IPv6 0xaef8b4c9765d2667      0t0  TCP localhost:redwood-broker (LISTEN)
ruby    6680 gdansk   13u  IPv4 0xaef8b4c96d351eef      0t0  TCP localhost:redwood-broker (LISTEN)
ruby    6680 gdansk   14u  IPv6 0xaef8b4c9765d2ee7      0t0  TCP localhost:redwood-broker (LISTEN)
```

とりあえず、起動はしててポートも使われている。

この状態でサーバを起動したマシンで `http://固定IP:3000` してもアクセスできない。

結局

`-b` オプションでhostを指定すればいい、デフォルトだと `localhost` にrails 4.2 で変更になったのだった。

```
% be rails s -p 3000 -b 0.0.0.0

% sudo lsof -i :3000
COMMAND  PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
ruby    6871 gdansk   12u  IPv4 0xaef8b4c960d84707      0t0  TCP *:redwood-broker (LISTEN)
```




```
$ be rails s -p 3000 -b 0.0.0.0
```

### 参考にした

- [rails sで起動したサーバにブラウザからアクセスできない \- Qiita](http://qiita.com/tmf16/items/bb789a45c0f456eac11f)
- [0\.0\.0\.0 ‐ 通信用語の基礎知識](http://www.wdic.org/w/WDIC/0.0.0.0)
- [127\.0\.0\.1 ‐ 通信用語の基礎知識](http://www.wdic.org/w/WDIC/127.0.0.1)