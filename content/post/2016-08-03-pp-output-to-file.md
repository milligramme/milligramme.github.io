+++
title = "pp出力をファイルに書き出したい"
date = "2016-08-03T18:21:00+09:00"
tags = ["ruby"]

outdated = false 
+++

`Object#pretty_inspect` 使えばいい

[ppの出力結果を文字列として格納する方法 \- 森薫の日記](http://kaorumori.hatenadiary.com/entry/20101030/1289148501)

```rb
require 'pp'

hello = {
  :world => [
    [1,2,3,4,5,6,7,8,9,10],
    [11,12,13,14,15,16,17,18,19,20],
    [21,22,23,24,25,26,27,28,29,30]
  ]
}

pp hello

# {:world=>
#   [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
#    [11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
#    [21, 22, 23, 24, 25, 26, 27, 28, 29, 30]]}


# ファイルに書き出し
File.write File.expand_path("~/Desktop/world.txt"), hello.pretty_inspect
```
