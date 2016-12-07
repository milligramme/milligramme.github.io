+++
title = "文字コード相互変換メモ"
date = "2012-03-03T00:00:00+09:00"
tags = ["ruby"]
+++

個人的にJavaScriptでよくつかう

* String.fromCharCode("0x0000")
* "xxxx".charCodeAt(n).toString(16)

をRubyで使うときのメモ

```ruby
# -*- coding:  utf-8 -*-
ary = (9000..10000).to_a

# chrかpack("U*")をつかう
ary.each{ |c| puts [ c.chr('utf-8'), c.to_s(16)].join(": ") }
ary.each{ |c| puts [[ c ].pack("U*"), c.to_s(16)].join(": ") }

strs = ary.inject(""){ |x, c| x + c.chr("utf-8") }

# ordかunpack("U*")をつかう
strs.split(//).each{ |u| puts [u.encode("utf-8").ord.to_s(16), u].join(": ") }
strs.split(//).each{ |u| puts [u.unpack("U*")[0].to_s(16), u].join(": ") }
```

`.pack("U*")` のときが Array ていうのをよく忘れる