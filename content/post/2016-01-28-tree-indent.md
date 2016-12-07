+++
title = "treeのインデント"
date = "2016-01-28T00:00:00+09:00"
tags = ["cli"]
+++

[The Tree Command for Linux Homepage](http://mama.indstate.edu/users/ice/tree/)

で階層出力する時に、特にオプションを設定しないと見た目にはかわらないのだけど、
インデントが `0xa0` NO-BREAK SPACE になっていて使いづらいので
`-A` オプションをつけて `0x20` SPACE にしてみた。

```
% tree customXml
customXml
├── _rels
│   └── item1.xml.rels
├── item1.xml
└── itemProps1.xml

% tree customXml -A
customXml
├── _rels
│   └── item1.xml.rels
├── item1.xml
└── itemProps1.xml

% tree --help | grep indent
  -i            Don't print indentation lines.
  -A            Print ANSI lines graphic indentation lines.
  -S            Print with CP437 (console) graphics indentation lines.
```

### 参考にした

[Manpage of TREE](http://mama.indstate.edu/users/ice/tree/tree.1.html)