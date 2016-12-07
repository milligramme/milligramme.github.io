+++
title = "gitのtagをリモートにpushする "
date = "2011-12-14T00:00:00+09:00"
tags = ["git"]
+++
ローカルでタグをつける

```
$ git tag v0.9
```

リモートに反映させる

```
$ git push origin v0.9
```

まとめてリモートに反映させる

```
$ git tag v1.0a
:
:
:
$ git tag v1.0b
:
:
$ git tag v1.0rc
```

リモートに push してないタグを全て反映させる

```
$ git push origin --tags
```

過去に遡ってタグをつける

```
$ git log --pretty=oneline

bc485f2e49740e7d2ad62e08b785117a3bb958d7 beta
ade7a116ecabd0623ca04ece5f5496c06e3c1c55 preview
e1fec3d88263a28402d70212a06da9a62fd19146 rc

$ git tag v1.0pre ade7a116
```

なるほど