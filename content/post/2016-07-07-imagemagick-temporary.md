+++
title = "ImageMagickの設定"
date = "2016-07-07T19:58:00+09:00"
tags = ["imagemagick"]
+++

rmagick(imagemagick)の処理で 130GBあったSSD残容量が 50GBまで減ってしまったのであわてて設定

[ImageMagickのピクセルキャッシュとリソース制限 \- クックパッド開発者ブログ](http://techlife.cookpad.com/entry/2016/05/18/180703)

```
% convert -version
Version: ImageMagick 6.9.3-7 Q16 x86_64 2016-05-03 http://www.imagemagick.org
Copyright: Copyright (C) 1999-2016 ImageMagick Studio LLC
License: http://www.imagemagick.org/script/license.php
Features: Cipher DPC Modules
Delegates (built-in): bzlib freetype jng jpeg ltdl lzma png tiff xml zlib
```

`/usr/local/Cellar/imagemagick/6.9.3-7_1/etc/ImageMagick-6/policy.xml` を編集

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE policymap [
<!ELEMENT policymap (policy)+>
<!ELEMENT policy (#PCDATA)>
<!ATTLIST policy domain (delegate|coder|filter|path|resource) #IMPLIED>
<!ATTLIST policy name CDATA #IMPLIED>
<!ATTLIST policy rights CDATA #IMPLIED>
<!ATTLIST policy pattern CDATA #IMPLIED>
<!ATTLIST policy value CDATA #IMPLIED>
]>
<!--
  Configure ImageMagick policies.

  Domains include system, delegate, coder, filter, path, or resource.

  Rights include none, read, write, and execute.  Use | to combine them,
  for example: "read | write" to permit read from, or write to, a path.

  Use a glob expression as a pattern.

  Suppose we do not want users to process MPEG video images:

    <policy domain="delegate" rights="none" pattern="mpeg:decode" />

  Here we do not want users reading images from HTTP:

    <policy domain="coder" rights="none" pattern="HTTP" />

  Lets prevent users from executing any image filters:

    <policy domain="filter" rights="none" pattern="*" />

  The /repository file system is restricted to read only.  We use a glob
  expression to match all paths that start with /repository:
  
    <policy domain="path" rights="read" pattern="/repository/*" />

  Any large image is cached to disk rather than memory:

    <policy domain="resource" name="area" value="1GB"/>

  Define arguments for the memory, map, area, width, height, and disk resources
  with SI prefixes (.e.g 100MB).  In addition, resource policies are maximums
  for each instance of ImageMagick (e.g. policy memory limit 1GB, -limit 2GB
  exceeds policy maximum so memory limit is 1GB).
-->
<policymap>
  <policy domain="resource" name="temporary-path" value="/tmp"/>
  <!-- <policy domain="resource" name="memory" value="2GiB"/> -->
  <!-- <policy domain="resource" name="map" value="4GiB"/> -->
  <!-- <policy domain="resource" name="width" value="10MP"/> -->
  <!-- <policy domain="resource" name="height" value="10MP"/> -->
  <!-- <policy domain="resource" name="area" value="1GB"/> -->
  <policy domain="resource" name="disk" value="1GB"/>
  <!-- <policy domain="resource" name="file" value="768"/> -->
  <!-- <policy domain="resource" name="thread" value="4"/> -->
  <!-- <policy domain="resource" name="throttle" value="0"/> -->
  <!-- <policy domain="resource" name="time" value="3600"/> -->
  <!-- <policy domain="system" name="precision" value="6"/> -->
  <policy domain="cache" name="shared-secret" value="passphrase"/>
  <policy domain="coder" rights="none" pattern="EPHEMERAL" />
  <policy domain="coder" rights="none" pattern="URL" />
  <policy domain="coder" rights="none" pattern="HTTPS" />
  <policy domain="coder" rights="none" pattern="MVG" />
  <policy domain="coder" rights="none" pattern="MSL" />
</policymap>
```


`temporary-path` と  `disk unlimited` となっていた箇所をコメントインして設定した
