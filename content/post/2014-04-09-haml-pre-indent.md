+++
title = "haml-pre-indent"
date = "2014-04-09T00:00:00+09:00"
tags = ["haml"]
+++

middlemanで（に限らず） hamlでpreタグ内が、2行目からインデントされてしまうのを直す

``` ruby
= preserve do
  = yield
```

`preserve do` ブロックに入れるといい

### 参考にした
- [How to remove unwanted intend from HAML's pre tag - Stack Overflow](http://stackoverflow.com/questions/1993993/how-to-remove-unwanted-intend-from-hamls-pre-tag)