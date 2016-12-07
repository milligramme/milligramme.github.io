+++
title = "middleman-blogの同日articleの問題を修正"
date = "2016-05-13T16:34:00+09:00"
tags = ["middleman"]
+++

同日の date があると `previous_article`, `next_article` の挙動がおかしかったのでしらべた。

[previous\_article, next\_article wrong results if more articles on same day · Issue \#241 · middleman/middleman\-blog](https://github.com/middleman/middleman-blog/issues/241)

Frontmatterのdate: が YYYY-MM-DD だったのをtime付に変更した


（jekyllからmiddlemanにしたときにパースエラーをおこして時間をとっていたような…）