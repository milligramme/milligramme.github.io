+++
title = "native extensionなgemを探す"
date = "2015-12-01T00:00:00+09:00"
tags = ["gem"]
+++

native extensionなgemを探す

[List gems with native extensions](https://gist.github.com/aelesbao/1414b169a79162b1d795)

```
irb(main):008:0> Gem::Specification.each.select{|spec|spec.extensions.any?}
[
    [ 0] #<Gem::Specification:0x3fe2b180b564 bigdecimal-1.2.6>,
    [ 1] #<Gem::Specification:0x3fe2b18128dc binding_of_caller-0.7.2>,
    [ 2] #<Gem::Specification:0x3fe2b1481218 byebug-6.0.2>,
    [ 3] #<Gem::Specification:0x3fe2b14708f0 debug_inspector-0.0.2>,
    [ 4] #<Gem::Specification:0x3fe2b145b0b8 eventmachine-1.0.8>,
    [ 5] #<Gem::Specification:0x3fe2b144b2d0 ffi-1.9.10>,
    [ 6] #<Gem::Specification:0x3fe2b1437f78 hpricot-0.8.6>,
    [ 7] #<Gem::Specification:0x3fe2b142a364 http_parser.rb-0.6.0>,
    [ 8] #<Gem::Specification:0x3fe2b180fb14 io-console-0.4.3>,
    [ 9] #<Gem::Specification:0x3fe2b141764c json-1.8.3>,
    [10] #<Gem::Specification:0x3fe2b10519a4 json-1.8.2>,
    [11] #<Gem::Specification:0x3fe2b10451cc libxml-ruby-2.8.0>,
    [12] #<Gem::Specification:0x3fe2b1039110 libxslt-ruby-1.1.1>,
    [13] #<Gem::Specification:0x3fe2b182ba6c nokogiri-1.6.6.2>,
    [14] #<Gem::Specification:0x3fe2b1494750 oj-2.13.1>,
    [15] #<Gem::Specification:0x3fe2b181aec4 psych-2.0.8>,
    [16] #<Gem::Specification:0x3fe2b144e0e8 rmagick-2.15.4>,
    [17] #<Gem::Specification:0x3fe2b10411d0 sqlite3-1.3.11>,
    [18] #<Gem::Specification:0x3fe2b102d9a0 sqlite3-1.3.10>,
    [19] #<Gem::Specification:0x3fe2b1054140 thin-1.6.4>,
    [20] #<Gem::Specification:0x3fe2b105d240 thin-1.5.1>,
    [21] #<Gem::Specification:0x3fe2b1453da4 unf_ext-0.0.7.1>,
    [22] #<Gem::Specification:0x3fe2b1488b30 yajl-ruby-1.1.0>
]
```
