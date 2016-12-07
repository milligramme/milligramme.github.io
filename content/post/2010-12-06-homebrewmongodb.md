+++
title = "MongoDBインストールメモ"
date = "2010-12-06T00:00:00+09:00"
tags = ["homebrew", "mongodb"]
+++
Homebrewで [Mongo DB](http://www.mongodb.org/) をインストールしてみました。

![mongodb.png](/images/2010/12/mongodb.png)


```bash
$ brew install mongodb
==> Downloading http://fastdl.mongodb.org/osx/mongodb-osx-x86_64-1.6.3.tgz
######################################################################## 100.0%
==> Caveats
If this is your first install, automatically load on login with:
    cp /usr/local/Cellar/mongodb/1.6.3-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

If this is an upgrade and you already have the org.mongodb.mongod.plist loaded:
    launchctl unload -w ~/Library/LaunchAgents/org.mongodb.mongod.plist
    cp /usr/local/Cellar/mongodb/1.6.3-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
    launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist

Or start it manually:
    mongod run --config /usr/local/Cellar/mongodb/1.6.3-x86_64/mongod.conf
==> Summary
/usr/local/Cellar/mongodb/1.6.3-x86_64: 16 files, 83M, built in 2 seconds
```


データベースのデフォルトの保存先として（なんでここ？）

```bash
$ mkdir -p /data/db
```

となっていたのだけど忘れてた、mongod.conf をみてみたら

```bash
# Store data in /usr/local/var/mongodb instead of the default /data/db
dbpath = /usr/local/var/mongodb

# Only accept local connections
bind_ip = 127.0.0.1
```

となっていて、試しに作成していたデータベースもそこにできていた。


### 自動ロードとログインの設定

```bash
$ cp /usr/local/Cellar/mongodb/1.6.3-x86_64/org.mongodb.mongod.plist ~/Library/LaunchAgents
$ launchctl load -w ~/Library/LaunchAgents/org.mongodb.mongod.plist
```

### mongo shellをためしてみる

```bash
$ mongo
MongoDB shell version: 1.6.3
connecting to: test
> exit
bye
```

チュートリアルを試す。
 [チュートリアル - Docs-Japanese - 10gen Confluence](http://www.mongodb.org/display/DOCSJP/%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB#%E3%83%81%E3%83%A5%E3%83%BC%E3%83%88%E3%83%AA%E3%82%A2%E3%83%AB-%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%81%AE%E5%8F%96%E5%BE%97) 

```bash
#MongoDBのJavaScriptシェル（mongo shell）を起動
$ mongo
MongoDB shell version: 1.6.3
connecting to: test
```

### DBを切り替え

```bash
> use mydb
switched to db mydb
```

### データをコレクションに挿入

```bash
> j = { name : "mongo" };
{ "name" : "mongo" }
> t = { x : 3 };
{ "x" : 3 }
> db.things.save(j);
> db.things.save(t);
> db.things.find();
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
{ "_id" : ObjectId("4cfc91a10effeb1b0d02edca"), "x" : 3 }
```

### もっと追加

```bash
> for (var i = 1; i <= 20; i++) db.things.save({x : 4, j : i});
> db.things.find();
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
{ "_id" : ObjectId("4cfc91a10effeb1b0d02edca"), "x" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "x" : 4, "j" : 1 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcc"), "x" : 4, "j" : 2 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "x" : 4, "j" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "x" : 4, "j" : 4 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcf"), "x" : 4, "j" : 5 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd0"), "x" : 4, "j" : 6 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd1"), "x" : 4, "j" : 7 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd2"), "x" : 4, "j" : 8 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd3"), "x" : 4, "j" : 9 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd4"), "x" : 4, "j" : 10 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd5"), "x" : 4, "j" : 11 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd6"), "x" : 4, "j" : 12 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd7"), "x" : 4, "j" : 13 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd8"), "x" : 4, "j" : 14 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd9"), "x" : 4, "j" : 15 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edda"), "x" : 4, "j" : 16 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddb"), "x" : 4, "j" : 17 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddc"), "x" : 4, "j" : 18 }
has more
```

### itショートカットコマンドで続きを表示

このシェルでは20件までしか表示されない

```bash
> it
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddd"), "x" : 4, "j" : 19 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edde"), "x" : 4, "j" : 20 }
```


### whileをつかってインテレート

find()メソッドはカーソルオブジェクト（コレクションのすべて）を返す

カーソルタイプのインテレート、hasNext()が true なら .next() で次のドキュメントを返す

```bash
> var cursor = db.things.find();                    
> while (cursor.hasNext()) printjson(cursor.next());
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
{ "_id" : ObjectId("4cfc91a10effeb1b0d02edca"), "x" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "x" : 4, "j" : 1 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcc"), "x" : 4, "j" : 2 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "x" : 4, "j" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "x" : 4, "j" : 4 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcf"), "x" : 4, "j" : 5 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd0"), "x" : 4, "j" : 6 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd1"), "x" : 4, "j" : 7 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd2"), "x" : 4, "j" : 8 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd3"), "x" : 4, "j" : 9 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd4"), "x" : 4, "j" : 10 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd5"), "x" : 4, "j" : 11 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd6"), "x" : 4, "j" : 12 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd7"), "x" : 4, "j" : 13 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd8"), "x" : 4, "j" : 14 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd9"), "x" : 4, "j" : 15 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edda"), "x" : 4, "j" : 16 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddb"), "x" : 4, "j" : 17 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddc"), "x" : 4, "j" : 18 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddd"), "x" : 4, "j" : 19 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edde"), "x" : 4, "j" : 20 }
```

### whileの代わりにforEach() をつかう方法

forEach(function) とfunctionを定義する必要がある

```bash
> db.things.find().forEach(printjson);
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
{ "_id" : ObjectId("4cfc91a10effeb1b0d02edca"), "x" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "x" : 4, "j" : 1 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcc"), "x" : 4, "j" : 2 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "x" : 4, "j" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "x" : 4, "j" : 4 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcf"), "x" : 4, "j" : 5 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd0"), "x" : 4, "j" : 6 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd1"), "x" : 4, "j" : 7 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd2"), "x" : 4, "j" : 8 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd3"), "x" : 4, "j" : 9 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd4"), "x" : 4, "j" : 10 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd5"), "x" : 4, "j" : 11 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd6"), "x" : 4, "j" : 12 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd7"), "x" : 4, "j" : 13 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd8"), "x" : 4, "j" : 14 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd9"), "x" : 4, "j" : 15 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edda"), "x" : 4, "j" : 16 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddb"), "x" : 4, "j" : 17 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddc"), "x" : 4, "j" : 18 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddd"), "x" : 4, "j" : 19 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edde"), "x" : 4, "j" : 20 }
```

### 配列のようにも使える

この方法で呼び出すとアクセスされたドキュメントがすべて（cursor[0]〜cursor[4]）

メモリーに読み込まれるので大きな結果の場合によろしくない

```bash
> var cursor = db.things.find();
> printjson(cursor[4]);
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "x" : 4, "j" : 3 }
```

### 本当の配列にして扱う

```bash
> var arr = db.things.find().toArray();
> arr[5];
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "x" : 4, "j" : 4 }
```

クエリーを作るということは、

「マッチするkeyとvalueの組を指す"クエリー ドキュメント"というドキュメントのを作る」ということ

SQLクエリー

```bash
SELECT * FROM things WHERE name="mongo"
```

mongo shellだと

```bash
> db.things.find({name:"mongo"}).forEach(printjson);
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
```

SQLクエリー

```bash
SELECT * FROM things WHERE x=4
```

mongo shellだと

```bash
> db.things.find({x:4}).forEach(printjson);
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "x" : 4, "j" : 1 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcc"), "x" : 4, "j" : 2 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "x" : 4, "j" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "x" : 4, "j" : 4 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcf"), "x" : 4, "j" : 5 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd0"), "x" : 4, "j" : 6 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd1"), "x" : 4, "j" : 7 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd2"), "x" : 4, "j" : 8 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd3"), "x" : 4, "j" : 9 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd4"), "x" : 4, "j" : 10 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd5"), "x" : 4, "j" : 11 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd6"), "x" : 4, "j" : 12 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd7"), "x" : 4, "j" : 13 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd8"), "x" : 4, "j" : 14 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd9"), "x" : 4, "j" : 15 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edda"), "x" : 4, "j" : 16 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddb"), "x" : 4, "j" : 17 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddc"), "x" : 4, "j" : 18 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddd"), "x" : 4, "j" : 19 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edde"), "x" : 4, "j" : 20 }
```

SQLクエリー

```bash
SELECT j FROM things WHERE x=4
```

mongo shellだと

```bash
> db.things.find({x:4},{j:true}).forEach(printjson);
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "j" : 1 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcc"), "j" : 2 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcd"), "j" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edce"), "j" : 4 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcf"), "j" : 5 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd0"), "j" : 6 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd1"), "j" : 7 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd2"), "j" : 8 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd3"), "j" : 9 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd4"), "j" : 10 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd5"), "j" : 11 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd6"), "j" : 12 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd7"), "j" : 13 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd8"), "j" : 14 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edd9"), "j" : 15 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edda"), "j" : 16 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddb"), "j" : 17 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddc"), "j" : 18 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02eddd"), "j" : 19 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edde"), "j" : 20 }
```

### findOneは最初のドキュメントを返す、無ければnullを返す

```bash
> printjson(db.things.findOne({name:"mongo"}));       
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
```

### limit( n )で結果を制限

```bash
> db.things.find().limit(3)                    
{ "_id" : ObjectId("4cfc91930effeb1b0d02edc9"), "name" : "mongo" }
{ "_id" : ObjectId("4cfc91a10effeb1b0d02edca"), "x" : 3 }
{ "_id" : ObjectId("4cfc92230effeb1b0d02edcb"), "x" : 4, "j" : 1 }
```

### functionを括弧無しで入力するとソース表示される

```bash
> printjson
function (x) {
    print(tojson(x));
}
```