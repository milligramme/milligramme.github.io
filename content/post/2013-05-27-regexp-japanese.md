+++
title = "だいたい日本語（japanese）にマッチする正規表現"
date = "2013-05-27T00:00:00+09:00"
tags = ["regex"]
+++

```ruby
# -*- coding: utf-8 -*-
str = "人々は三〇三号室で $.appEncoding を指定するニャンw♪、あぅ〻"

p str.scan(/[^一-龠ぁ-ん]+/)
p str.scan(/[^々〇〻一-龠ぁ-ん]+/)
p str.scan(/[^々〇〻一-龠　-〽ぁ-ゟ゠-ヿ]+/)

# ["々", "〇", " $.appEncoding ", "ニャンw♪、", "〻"]
# [" $.appEncoding ", "ニャンw♪、"]
# [" $.appEncoding ", "w♪"]
```


よくある漢字 「一-龠」 だと々、〇、〻がぬける
