+++
title = "rsync でバックアップ"
date = "2013-11-27T00:00:00+09:00"
tags = ["rsync", "ssh"]
+++

```bash
# -a, --archive               archive mode; same as -rlptgoD (no -H)
# -v, --verbose               increase verbosity
#     --delete                delete extraneous files from destination dirs
# -e, --rsh=COMMAND           specify the remote shell to use

# リモートホストに同期させる
$ rsync -av --delete -e ssh SRC USER@HOST:DEST
```

スラッシュ有り無しによる挙動の違い

```bash
# src/
#   file
#   dir
#   
# スラッシュあり
# src/  dest
# =>
# dest/
#   file
#   dir
# 
# スラッシュなし
# src   dest
# =>
# dest/
#   src/
#     file
#     dir
```

不安なら `--dry-run` オプションで試すのがよさそう