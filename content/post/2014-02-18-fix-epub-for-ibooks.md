+++
title = "iBooksの日本語横組みepubが横スクロールになるのを直す"
date = "2014-02-18T00:00:00+09:00"
tags = ["xml", "epub"]

outdated = true
+++

iOS の iBooks の日本語横組み epub が横スクロールになるのを直す
 .epub ファイル内 `./OEBPS/xxxxxx.opf` を編集する

```xml
<package version="2.0" xmlns="http://www.idpf.org/2007/opf" unique-identifier="BookId">
  <metadata xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:opf="http://www.idpf.org/2007/opf">
    <dc:language>ja</dc:language>
```

を

```xml
<package version="2.0" xmlns="http://www.idpf.org/2007/opf" unique-identifier="BookId">
  <metadata xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:opf="http://www.idpf.org/2007/opf">
    <dc:language>en</dc:language>

```

dc:languageの `ja` を `en` になおしたらいい