+++
title = "mogrifyでバッチ処理"
date = "2013-11-14T00:00:00+09:00"
tags = ["cli", "osx", "imagemagick"]
+++

jpegをeps変換するのに ImageMagick の `mogrify` をつかった

```bash
$ mogrify -format eps *.jpg
```