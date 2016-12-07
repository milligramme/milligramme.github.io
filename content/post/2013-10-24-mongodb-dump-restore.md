+++
title = "mongodbでダンプとリストア"
date = "2013-10-24T00:00:00+09:00"
tags = ["mongodb"]

outdated = true
+++

mongodb のダンプとリストアのメモ

### ダンプ

```bash
$ mongodump --host localhost --db DBNAME
```

./dump/DBNAME ができて、jsonとbsonがダンプされる

### リストア

```bash
$ mongorestore --host localhost --db DBNAME /path/to/dump
```

### 参考にした

- [Backup and Restore with MongoDB Tools — MongoDB Manual 2.4.7](http://docs.mongodb.org/manual/tutorial/backup-databases-with-binary-database-dumps/)