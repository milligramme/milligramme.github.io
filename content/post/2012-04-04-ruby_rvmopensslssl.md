+++
title = "rvmでいれたopensslのsslエラーを回避"
date = "2012-04-04T00:00:00+09:00"
tags = ["ruby", "rvm"]

outdated = true
+++



[gisty](https://github.com/swdyh/gisty) をいれて、sync や postをしてみようと思ったのだけど、

`SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (OpenSSL::SSL::SSLError)` というエラーがでてこまった

openssl絡みでcert fileがないっぽいのだけどいままで放置していた件、やっと解決。

cert fileあるべき場所を

```
ruby -ropenssl -e "p OpenSSL::X509::DEFAULT_CERT_FILE"

#=> /Users/xxxxxx/.rvm/usr/ssl/cert.pem
```

で調べると、rvm!

該当のパスをみても cert.pem はなく、[http://curl.haxx.se/ca/cacert.pem](http://curl.haxx.se/ca/cacert.pem) をcurlのサイトからダウンロードしてきて、先程のパスにリネームしてコピー

### 参考にした

*   [GitHubにおけるSSLの認証エラーを回避するため、EV SSL用ルート証明書を追加する - 祈れ、そして働け ～ Ora et labora](http://d.hatena.ne.jp/tetsuyai/20110924/1316877887)
*   [ruby on rails - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed - Stack Overflow](http://stackoverflow.com/questions/4528101/ssl-connect-returned-1-errno-0-state-sslv3-read-server-certificate-b-certificat)