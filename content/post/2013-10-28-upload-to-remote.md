+++
title = "upload-to-remote"
date = "2013-10-28T00:00:00+09:00"
tags = ["cli"]
+++

リモートサーバに ssh でファイルのアップロード


```bash
$ scp [options] file user@host:file_path
```

カレントディレクトリの `file` をリモートの `file_path` にアップロードする

- -p アクセス権などの属性保持
- -r 再帰的にコピ

### 参考にした
- [bash - How to upload a file from the command line with FTP or SSH? - Super User](http://superuser.com/questions/82445/how-to-upload-a-file-from-the-command-line-with-ftp-or-ssh)