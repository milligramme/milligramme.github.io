+++
title = "Pry で what_methods をつかう"
date = "2012-01-26T00:00:00+09:00"
tags = ["ruby"]
+++

irb のかわりに Pryを使う。

そしてrubyでアレなんてメソッドだっけ？を解決する what_methods を使ってみた

```
gem install pry
gem install what_methods
```

~/.pryrcを作成し

```
require "what_methods"
```

使い方

```ruby
gdansk: ~ ♣  pry
[1] pry(main)> (0..3).what? [0,1,2,3]
[1] pry(#<Range>)> exit
0..3.to_a == [0, 1, 2, 3]
0..3.entries == [0, 1, 2, 3]
0..3.sort == [0, 1, 2, 3]
=> [:to_a, :entries, :sort]
[2] pry(main)> ["a",nil,"b",nil].what? ["a","b"]
[1] pry(#<Array>)> exit
["a", nil, "b", nil].compact == ["a", "b"]
["a", nil, "b", nil].compact! == ["a", "b"]
=> [:compact, :compact!]
[3] pry(main)> " aaa ".what? "aaa"
[1] pry(" aaa ")> exit
" aaa ".strip == "aaa"
" aaa ".strip! == "aaa"
=> [:strip, :strip!]
```

一度、pry(main) から pry(#<Class>)> となって exit してからでないと結果がでなかったのだけどなにか設定の仕方があるんだろうか？

irb + what_methods だとすぐ結果がでる

```ruby
ruby-1.9.3-p0 :001 > (0..3).what? [0,1,2,3]
0..3.to_a == [0, 1, 2, 3]
0..3.entries == [0, 1, 2, 3]
0..3.sort == [0, 1, 2, 3]
 => [:to_a, :entries, :sort] 
```

### 参考にした

*   [Rubyでアレするには何のメソッド使えばいいのか悩んだ時『what_methods』 - 牌語備忘録 - pygo](http://d.hatena.ne.jp/CortYuming/20120115/p1)
*   [Pry - an IRB alternative and runtime developer console](http://pry.github.com/)