+++
date = "2013-09-26T00:00:00+09:00"
title = "Heroku PGの1万行制限が近づいてきた"
tags = ["heroku", "stringer"]

outdated = true
+++

### やりたいこと

Heroku で使ってる RSSリーダ Stringer のDBのDev(フリー)プランの制限 1万行が近づいてきた（7000くらい）のでなんとかする。
たぶん既読のDB行を削除していったらよさそうな気がする。

heroku pg コマンドを確認

```bash
$ heroku pg --help

 List databases for an app

Additional commands, type "heroku help COMMAND" for more details:

  pg:credentials DATABASE     #  Display the DATABASE credentials.
  pg:info [DATABASE]          #
  pg:kill procpid [DATABASE]  #  kill a query
  pg:killall [DATABASE]       #  terminates ALL connections
  pg:promote DATABASE         #  Sets DATABASE as your DATABASE_URL
  pg:ps [DATABASE]            #  view active queries with execution time
  pg:psql [DATABASE]          #  Open a psql shell to the database
  pg:reset DATABASE           #  Delete all data in DATABASE
  pg:unfollow REPLICA         #  stop a replica from following and make it a read/write database
  pg:wait [DATABASE]          #  monitor database creation, exit when complete
```

#### 使用率を確認

7000行強...

```bash
$ heroku pg
=== HEROKU_POSTGRESQL_COLOR_URL (DATABASE_URL)
Plan:        Dev
Status:      available
Connections: 2
PG Version:  9.2.4
Created:     2013-07-18 01:26 UTC
Data Size:   13.9 MB
Tables:      5
Rows:        7030/10000 (In compliance, close to row limit)
Fork/Follow: Unsupported
```

#### Stringer の rake cleanup_old_stories 実行

`rake cleanup_old_stories` をためしてみて、確かに既読 stories は消えたけど、再び `heroku pg` してみると rows が全然減ってない。

```bash
$ heroku run rake cleanup_old_stories[20]
Running `rake cleanup_old_stories[20]` attached to terminal... up, run.7044
** Building /js/application.js...
** Building /css/application.css...
D, [2013-09-25T16:43:06.648308 #2] DEBUG -- :   SQL (1701.3ms)  DELETE FROM "stories" WHERE "stories"."is_read" = 't' AND "stories"."is_starred" = 'f' AND (published <= '2013-09-05 16:43:04.921631')

$ heroku pg
=== HEROKU_POSTGRESQL_COLOR_URL (DATABASE_URL)
Plan:        Dev
Status:      available
Connections: 2
PG Version:  9.2.4
Created:     2013-07-18 01:26 UTC
Data Size:   13.9 MB
Tables:      5
Rows:        7030/10000 (In compliance, close to row limit)
Fork/Follow: Unsupported
```

### heroku pg コマンドを拡張して heroku pg:vacuum_stats で吸う？

調べてみて、 [Heroku Postgres Database Tuning \| Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-postgres-database-tuning#determining-bloat) できそうな気がする

[heroku/heroku-pg-extras](https://github.com/heroku/heroku-pg-extras)  をインストールして `heroku pg` コマンドを拡張

```bash
$ heroku plugins:install git://github.com/heroku/heroku-pg-extras.git
Installing heroku-pg-extras... done

$ heroku pg --help
Usage: heroku pg

 List databases for an app

Additional commands, type "heroku help COMMAND" for more details:

  pg:bloat [DATABASE]                                       #  show table and index bloat in your database ordered by most wasteful
  pg:blocking [DATABASE]                                    #  display queries holding locks other queries are waiting to be released
  pg:cache_hit [DATABASE]                                   #  calculates your cache hit rate (effective databases are at 99% and up)
  pg:cachehit                                               #
  pg:credentials DATABASE                                   #  Display the DATABASE credentials.
  pg:extensions [DATABASE]                                  #  List available and installed extensions.
  pg:index_size [DATABASE]                                  #  show the size of indexes, descending by size
  pg:index_usage [DATABASE]                                 #  calculates your index hit rate (effective databases are at 99% and up)
  pg:indexusage                                             #
  pg:info [DATABASE]                                        #
  pg:kill procpid [DATABASE]                                #  kill a query
  pg:killall [DATABASE]                                     #  terminates ALL connections
  pg:locks [DATABASE]                                       #  display queries with active locks
  pg:long_running_queries [DATABASE]                        #  show all queries taking longer than five minutes ordered by duration
  pg:mandelbrot [DATABASE]                                  #  show the mandelbrot set
  pg:outliers                                               #  Show queries that potentially need to be optimized.
  pg:promote DATABASE                                       #  Sets DATABASE as your DATABASE_URL
  pg:ps [DATABASE]                                          #  view active queries with execution time
  pg:psql [DATABASE]                                        #  Open a psql shell to the database
  pg:pull <REMOTE_SOURCE_DATABASE> <LOCAL_TARGET_NAME>      #  Pull from REMOTE_SOURCE_DATABASE to LOCAL_TARGET_NAME
  pg:push <LOCAL_SOURCE_DATABASE> <REMOTE_TARGET_DATABASE>  #  Push from LOCAL_SOURCE_DATABASE to REMOTE_TARGET_DATABASE
  pg:reset DATABASE                                         #  Delete all data in DATABASE
  pg:seq_scans [DATABASE]                                   #  show the count of seq_scans by table descending by order
  pg:total_index_size [DATABASE]                            #  show the total size of all indexes in MB
  pg:unfollow REPLICA                                       #  stop a replica from following and make it a read/write database
  pg:unused_indexes [DATABASE]                              #  Show unused and almost unused indexes, ordered by their size relative to
  pg:vacuum_stats [DATABASE]                                #  Show dead rows and whether an automatic vacuum is expected to be triggered
  pg:wait [DATABASE]                                        #  monitor database creation, exit when complete
```

#### 肥大してる箇所をさがす pg:bloat

最初に使うとき `pg-extras` の使用状況をトラックしていいか？と訊いてくるので yes 、デフォルトは no

```bash
$ heroku pg:bloat HEROKU_POSTGRESQL_COLOR_URL --app my-stringer-app
 !    Attention!
Hello! We at Heroku would like to track your usage of pg-extras.
This data helps us direct our efforts in supporting and adopting the
features used here. All data is anonymous; we only collect the command name,
nothing else.

Please answer (y/n) if this is OK. A file is written to /Users/gdansk/.heroku/pg-extras.conf recording
your reply. Default is "no".
 (n/y): y
 type  | schemaname |                  object_name                   | bloat |   waste
-------+------------+------------------------------------------------+-------+------------
 table | pg_catalog | pg_shdepend                                    |   2.2 | 1368 kB
 table | pg_catalog | pg_database                                    |   2.7 | 1232 kB
 index | pg_catalog | pg_shdepend::pg_shdepend_depender_index        |   1.8 | 792 kB
 table | public     | stories                                        |   1.2 | 704 kB
 index | pg_catalog | pg_shdepend::pg_shdepend_reference_index       |   1.2 | 224 kB
 table | pg_catalog | pg_rewrite                                     |   6.0 | 80 kB
 table | pg_catalog | pg_depend                                      |   1.1 | 24 kB
 index | pg_catalog | pg_amproc::pg_amproc_fam_proc_index            |   2.0 | 16 kB
 index | public     | feeds::index_feeds_on_url                      |   2.0 | 16 kB
 index | pg_catalog | pg_amop::pg_amop_opr_fam_index                 |   2.0 | 16 kB
 index | pg_catalog | pg_ts_config_map::pg_ts_config_map_index       |   2.0 | 16 kB
 index | pg_catalog | pg_amop::pg_amop_fam_strat_index               |   2.0 | 16 kB
 index | pg_catalog | pg_amop::pg_amop_oid_index                     |   2.0 | 16 kB
 table | public     | feeds                                          |   1.7 | 16 kB
 table | pg_catalog | pg_collation                                   |   1.1 | 16 kB
 table | pg_catalog | pg_opfamily                                    |   2.0 | 8192 bytes
 index | pg_catalog | pg_am::pg_am_name_index                        |   2.0 | 8192 bytes
 index | pg_catalog | pg_am::pg_am_oid_index                         |   2.0 | 8192 bytes
 index | pg_catalog | pg_cast::pg_cast_oid_index                     |   2.0 | 8192 bytes
 index | pg_catalog | pg_cast::pg_cast_source_target_index           |   2.0 | 8192 bytes
 index | pg_catalog | pg_constraint::pg_constraint_conname_nsp_index |   2.0 | 8192 bytes
 index | pg_catalog | pg_constraint::pg_constraint_conrelid_index    |   2.0 | 8192 bytes
 index | pg_catalog | pg_constraint::pg_constraint_contypid_index    |   2.0 | 8192 bytes
 index | pg_catalog | pg_constraint::pg_constraint_oid_index         |   2.0 | 8192 bytes
 index | pg_catalog | pg_extension::pg_extension_name_index          |   2.0 | 8192 bytes
 index | pg_catalog | pg_extension::pg_extension_oid_index           |   2.0 | 8192 bytes
 index | pg_catalog | pg_language::pg_language_name_index            |   2.0 | 8192 bytes
 index | pg_catalog | pg_language::pg_language_oid_index             |   2.0 | 8192 bytes
 index | pg_catalog | pg_namespace::pg_namespace_nspname_index       |   2.0 | 8192 bytes
 index | pg_catalog | pg_namespace::pg_namespace_oid_index           |   2.0 | 8192 bytes
 index | pg_catalog | pg_opfamily::pg_opfamily_am_name_nsp_index     |   2.0 | 8192 bytes
 index | pg_catalog | pg_opfamily::pg_opfamily_oid_index             |   2.0 | 8192 bytes
 index | pg_catalog | pg_pltemplate::pg_pltemplate_name_index        |   2.0 | 8192 bytes
 index | pg_catalog | pg_range::pg_range_rngtypid_index              |   2.0 | 8192 bytes
 index | pg_catalog | pg_shdescription::pg_shdescription_o_c_index   |   2.0 | 8192 bytes
 index | pg_catalog | pg_tablespace::pg_tablespace_oid_index         |   2.0 | 8192 bytes
 index | pg_catalog | pg_tablespace::pg_tablespace_spcname_index     |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_config::pg_ts_config_cfgname_index       |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_config::pg_ts_config_oid_index           |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_dict::pg_ts_dict_dictname_index          |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_dict::pg_ts_dict_oid_index               |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_parser::pg_ts_parser_oid_index           |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_parser::pg_ts_parser_prsname_index       |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_template::pg_ts_template_oid_index       |   2.0 | 8192 bytes
 index | pg_catalog | pg_ts_template::pg_ts_template_tmplname_index  |   2.0 | 8192 bytes
 index | pg_catalog | pg_aggregate::pg_aggregate_fnoid_index         |   2.0 | 8192 bytes
 table | pg_catalog | pg_amproc                                      |   1.5 | 8192 bytes
 table | pg_catalog | pg_conversion                                  |   1.5 | 8192 bytes
 table | pg_catalog | pg_amop                                        |   1.3 | 8192 bytes
 table | pg_catalog | pg_operator                                    |   1.1 | 8192 bytes
 table | pg_catalog | pg_description                                 |   1.0 | 8192 bytes
 table | pg_catalog | pg_type                                        |   1.0 | 0 bytes
 index | pg_catalog | pg_amproc::pg_amproc_oid_index                 |   1.0 | 0 bytes
 index | pg_catalog | pg_conversion::pg_conversion_default_index     |   1.0 | 0 bytes
 table | pg_catalog | pg_extension                                   |   1.0 | 0 bytes
 table | pg_catalog | pg_index                                       |   1.0 | 0 bytes
 table | pg_catalog | pg_language                                    |   1.0 | 0 bytes
 table | pg_catalog | pg_namespace                                   |   1.0 | 0 bytes
 table | pg_catalog | pg_opclass                                     |   1.0 | 0 bytes
 table | pg_catalog | pg_ts_parser                                   |   1.0 | 0 bytes
 table | pg_catalog | pg_pltemplate                                  |   1.0 | 0 bytes
 index | public     | feeds::feeds_pkey                              |   1.0 | 0 bytes
 table | pg_catalog | pg_ts_template                                 |   1.0 | 0 bytes
 table | pg_catalog | pg_ts_config_map                               |   1.0 | 0 bytes
 index | pg_catalog | pg_depend::pg_depend_depender_index            |   1.0 | 0 bytes
 table | pg_catalog | pg_range                                       |   1.0 | 0 bytes
 table | pg_catalog | pg_proc                                        |   1.0 | 0 bytes
 index | pg_catalog | pg_index::pg_index_indexrelid_index            |   1.0 | 0 bytes
 index | pg_catalog | pg_index::pg_index_indrelid_index              |   1.0 | 0 bytes
 table | pg_catalog | pg_aggregate                                   |   1.0 | 0 bytes
 table | pg_catalog | pg_am                                          |   1.0 | 0 bytes
 table | pg_catalog | pg_attribute                                   |   1.0 | 0 bytes
 index | pg_catalog | pg_rewrite::pg_rewrite_rel_rulename_index      |   1.0 | 0 bytes
 index | pg_catalog | pg_conversion::pg_conversion_oid_index         |   1.0 | 0 bytes
 index | pg_catalog | pg_conversion::pg_conversion_name_nsp_index    |   1.0 | 0 bytes
 table | pg_catalog | pg_constraint                                  |   1.0 | 0 bytes
 table | pg_catalog | pg_class                                       |   1.0 | 0 bytes
 table | pg_catalog | pg_shdescription                               |   1.0 | 0 bytes
 table | pg_catalog | pg_ts_dict                                     |   1.0 | 0 bytes
 index | pg_catalog | pg_opclass::pg_opclass_am_name_nsp_index       |   1.0 | 0 bytes
 index | pg_catalog | pg_opclass::pg_opclass_oid_index               |   1.0 | 0 bytes
 table | pg_catalog | pg_ts_config                                   |   1.0 | 0 bytes
 table | pg_catalog | pg_tablespace                                  |   1.0 | 0 bytes
 index | pg_catalog | pg_rewrite::pg_rewrite_oid_index               |   1.0 | 0 bytes
 table | pg_catalog | pg_cast                                        |   1.0 | 0 bytes
 index | pg_catalog | pg_depend::pg_depend_reference_index           |   0.9 | 0 bytes
 index | pg_catalog | pg_description::pg_description_o_c_o_index     |   0.8 | 0 bytes
 index | pg_catalog | pg_class::pg_class_relname_nsp_index           |   0.7 | 0 bytes
 index | pg_catalog | pg_type::pg_type_typname_nsp_index             |   0.6 | 0 bytes
 index | pg_catalog | pg_proc::pg_proc_proname_args_nsp_index        |   0.5 | 0 bytes
 index | pg_catalog | pg_operator::pg_operator_oprname_l_r_n_index   |   0.5 | 0 bytes
 index | pg_catalog | pg_operator::pg_operator_oid_index             |   0.4 | 0 bytes
 index | pg_catalog | pg_attribute::pg_attribute_relid_attnam_index  |   0.4 | 0 bytes
 index | pg_catalog | pg_database::pg_database_datname_index         |   0.3 | 0 bytes
 index | pg_catalog | pg_collation::pg_collation_name_enc_nsp_index  |   0.3 | 0 bytes
 index | pg_catalog | pg_type::pg_type_oid_index                     |   0.3 | 0 bytes
 index | pg_catalog | pg_class::pg_class_oid_index                   |   0.3 | 0 bytes
 index | pg_catalog | pg_attribute::pg_attribute_relid_attnum_index  |   0.3 | 0 bytes
 index | public     | stories::index_stories_on_entry_id_and_feed_id |   0.3 | 0 bytes
 index | pg_catalog | pg_database::pg_database_oid_index             |   0.2 | 0 bytes
 index | pg_catalog | pg_proc::pg_proc_oid_index                     |   0.2 | 0 bytes
 index | pg_catalog | pg_collation::pg_collation_oid_index           |   0.2 | 0 bytes
 index | public     | stories::stories_pkey                          |   0.1 | 0 bytes
(103 rows)
```

さきほどの pg-extras.conf に書き込まれた設定

```bash
$ cat /Users/gdansk/.heroku/pg-extras.conf
---
:collect_stats: true
```

### 吸い取る pg:vacuum_stats 

みるみる吸い取られます

```bash
$ heroku pg:vacuum_stats HEROKU_POSTGRESQL_COLOR_URL --app my-stringer-app
 schema |       table       | last_vacuum | last_autovacuum  |    rowcount    | dead_rowcount  | autovacuum_threshold | expect_autovacuum
--------+-------------------+-------------+------------------+----------------+----------------+----------------------+-------------------
 public | schema_migrations |             |                  |              0 |              0 |             50       |
 public | stories           |             | 2013-09-25 04:57 |          3,076 |              0 |            665       |
 public | delayed_jobs      |             |                  |              0 |             10 |             50       |
 public | feeds             |             | 2013-09-25 01:45 |            138 |             39 |             78       |
 public | users             |             |                  |              0 |              2 |             50       |
(5 rows)


### 確認

4000行弱吸った

$ heroku pg:info
=== HEROKU_POSTGRESQL_COLOR_URL (DATABASE_URL)
Plan:        Dev
Status:      available
Connections: 2
PG Version:  9.2.4
Created:     2013-07-18 01:26 UTC
Data Size:   14.1 MB
Tables:      5
Rows:        3214/10000 (In compliance)
Fork/Follow: Unsupported
```

結果的にやりたいことができたけど、あってる使い方かどうかちょっと自信ない。

あと `autovacuum` というのも気になるので今度調べてみる。

まとめると

```bash
#!/usr/bin/env bash

db_url=HEROKU_POSTGRESQL_COLOR_URL
app_name=my-stringer-app

cd /path/to/stringer-app
heroku pg
heroku pg:bloat $db_url --app $app_name
heroku pg:vacuum_stats $db_url --app $app_name
heroku pg
```
