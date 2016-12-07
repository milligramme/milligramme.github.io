+++
title = "postgresql-on-osx"
date = "2014-08-25T00:00:00+09:00"
tags = ["osx", "homebrew", "postgresql"]
+++

[Stringer](https://github.com/swanson/stringer) でdevelepment環境もPostgreSQLが必要なためインストール

[install PostgreSQL 9 in Mac OSX via Homebrew](https://gist.github.com/lxneng/741932)

```
# install the binary
$ brew install postgresql

# init it
$ initdb /usr/local/var/postgres

# start the postgres server
$ postgres -D /usr/local/var/postgres

# create your database
$ createdb stringer_dev
```

サーバ起動時にエラーでたのを対応

["/var/pgsql_socket/.s.PGSQL.5432"?｜linux-555](http://ameblo.jp/soft3133/entry-11466406890.html)

```
$ sudo mkdir /var/pgsql_socket/
$ sudo ln -s /private/tmp/.s.PGSQL.5432 /var/pgsql_socket
```

rake db:migrate したら db/schema.rb で `null: false` が消えた

```diff
% git diff
diff --git a/db/schema.rb b/db/schema.rb
index 18ff40d..69ba2a3 100644
--- a/db/schema.rb
+++ b/db/schema.rb
@@ -25,8 +25,8 @@ ActiveRecord::Schema.define(version: 20140421224454) do
     t.datetime "failed_at"
     t.string   "locked_by"
     t.string   "queue"
-    t.datetime "created_at",             null: false
-    t.datetime "updated_at",             null: false
+    t.datetime "created_at"
+    t.datetime "updated_at"
   end

   add_index "delayed_jobs", ["priority", "run_at"], name: "delayed_jobs_priority", using: :btree
@@ -35,8 +35,8 @@ ActiveRecord::Schema.define(version: 20140421224454) do
     t.string   "name"
     t.text     "url"
     t.datetime "last_fetched"
-    t.datetime "created_at",   null: false
-    t.datetime "updated_at",   null: false
+    t.datetime "created_at"
+    t.datetime "updated_at"
     t.integer  "status"
     t.integer  "group_id"
   end
@@ -54,8 +54,8 @@ ActiveRecord::Schema.define(version: 20140421224454) do
     t.text     "permalink"
     t.text     "body"
     t.integer  "feed_id"
-    t.datetime "created_at",                  null: false
-    t.datetime "updated_at",                  null: false
+    t.datetime "created_at"
+    t.datetime "updated_at"
     t.datetime "published"
     t.boolean  "is_read"
     t.boolean  "keep_unread", default: false
@@ -67,8 +67,8 @@ ActiveRecord::Schema.define(version: 20140421224454) do

   create_table "users", force: true do |t|
     t.string   "password_digest"
-    t.datetime "created_at",      null: false
-    t.datetime "updated_at",      null: false
+    t.datetime "created_at"
+    t.datetime "updated_at"
     t.boolean  "setup_complete"
     t.string   "api_key"
   end
```

not nullが...

```
% psql stringer_dev
stringer_dev=# \d delayed_jobs
                                     Table "public.delayed_jobs"
   Column   |            Type             |                         Modifiers
------------+-----------------------------+-----------------------------------------------------------
 id         | integer                     | not null default nextval('delayed_jobs_id_seq'::regclass)
 priority   | integer                     | default 0
 attempts   | integer                     | default 0
 handler    | text                        |
 last_error | text                        |
 run_at     | timestamp without time zone |
 locked_at  | timestamp without time zone |
 failed_at  | timestamp without time zone |
 locked_by  | character varying(255)      |
 queue      | character varying(255)      |
 created_at | timestamp without time zone |
 updated_at | timestamp without time zone |
Indexes:
    "delayed_jobs_pkey" PRIMARY KEY, btree (id)
    "delayed_jobs_priority" btree (priority, run_at)
```
