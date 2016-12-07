+++
title = "github-pages-build-failed"
date = "2013-07-24T00:00:00+09:00"
tags = ["jekyll", "githubpages"]

outdated = true
+++

github pages を jekyll でよしなにビルドしてもらっているのだけど、たまに失敗してメールがくる

```
The page build failed with the following error:

page build failed

For information on troubleshooting Jekyll see https://help.github.com/articles/using-jekyll-with-pages#troubleshooting
If you have any questions please contact GitHub Support.

support@github.com
https://github.com/contact
``` 
    
失敗したのは分けるけど、ヒントがあまりにないので、問題になったところをメモしていく

* 文中に行頭のコロン (:)
* 文中に閉じてないバッククオート（｀） -> ｀.DS\_Store'

また遭遇した追加する

* Gemfile 内の gem のバージョンがしょっちゅう変わって、合わせないとエラーになる