+++
title = "はじめてのglitch"
date = "2012-02-14T00:00:00+09:00"
tags = ["ruby"]
+++

はじめての glitch

```ruby
# -*- coding:  utf-8 -*-
source = ARGV.first
w = %w(a b c d e f g h i j k l)
10.times do |i|
  `cat #{source} | sed 's/\s/#{i}/g' > #{source.sub(/\./,"0#{i}.")}`
  `cat #{source} | sed 's/\d/#{i}/g' > #{source.sub(/\./,"1#{i}.")}`
  `cat #{source} | sed 's/[a-z]/#{i}/g' > #{source.sub(/\./,"2#{i}.")}`
  `cat #{source} | sed 's/\w/#{i}/g' > #{source.sub(/\./,"3#{i}.")}`
  `cat #{source} | sed 's/#{i}/w[#{i}]/g' > #{source.sub(/\./,"4#{i}.")}`
end
```

### png

[![](/images/2012/02/fuglitch_png.gif "fuglitch_png")](/images/2012/02/fuglitch_png.gif)

### jpg

[![](/images/2012/02/fuglitch_jpg.gif "fuglitch_jpg")](/images/2012/02/fuglitch_jpg.gif)