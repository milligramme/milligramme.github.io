+++
title = "FileUtilsのcp_rとかcopy_entry"
date = "2013-07-17T00:00:00+09:00"
tags = ["ruby"]
+++

FileUtils.cp\_r とか FileUtils.copy_entry

     Desktop/EQ/
     └── plugin
         ├── rt_fav_count.rb
         ├── show_plugins.rb
         └── xxx
             ├── bitly_clicks.rb
             └── debug.rb

こんなpluginのなかみをバックアップフォルダ（cp\_r）にコピーしたい。
cp\_rディレクトリが存在しない場合はうまくいくけど、存在する場合 cp\_r/plugin
となってしまう。

> remove\_destination: 真を指定するとコピーを実行する前にコピー先を削除します。

で cp\_r フォルダを削除してくれるのを期待したら違ったっぽい


```rb
require "fileutils"

src            = File.expand_path('~/Desktop/EQ/plugin')
dst_copy_entry = File.expand_path('~/Desktop/EQ/copy_entry')
dst_cp_r       = File.expand_path('~/Desktop/EQ/cp_r')

FileUtils.cp_r src, dst_cp_r, remove_destination: true

# 1回目
# ├── cp_r
# │ ├── rt_fav_count.rb
# │ ├── show_plugins.rb
# │ └── xxx
# │     ├── bitly_clicks.rb
# │     └── debug.rb

# 2回目
# ├── cp_r
# │ ├── plugin
# │ │ ├── rt_fav_count.rb
# │ │ ├── show_plugins.rb
# │ │ └── xxx
# │ │     ├── bitly_clicks.rb
# │ │     └── debug.rb
# │ ├── rt_fav_count.rb
# │ ├── show_plugins.rb
# │ └── xxx
# │     ├── bitly_clicks.rb
# │     └── debug.rb
```

`:remove_destination => true` オプションはコピー先のディレクトリを削除する訳ではないみたいので、毎回削除してからコピーせな

```rb
FileUtils.rm_r dst_cp_r, force: true if File.exist?(dst_cp_r)
FileUtils.cp_r src, dst_cp_r

# ├── cp_r
# │ ├── rt_fav_count.rb
# │ ├── show_plugins.rb
# │ └── xxx
# │     ├── bitly_clicks.rb
# │     └── debug.rb
```

FileUtils.copy\_entry も一見よさそうだったのだけど src で削除したファイルの削除といった面倒までみてくれない

```rb
FileUtils.copy_entry src, dst_copy_entry

# ├── copy_entry
# │ ├── delete_all_tweets.rb いらないもコピーされてる
# │ ├── rt_fav_count.rb
# │ ├── show_plugins.rb
# │ └── xxx
# │     ├── bitly_clicks.rb
# │     └── debug.rb
```