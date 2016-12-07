+++
title = "jsx & underscore.js は全部のメソッドが動くとは限らない"
date = "2014-07-28T00:00:00+09:00"
tags = ["extendscript", "underscore.js"]
+++

jsx だと `Math.max` `Math.min` が2引数しか扱わないので、配列の最初と最後しかみてないので

`_.max` と `_.min` が想定外の結果を返すのを思い出した

<script src="https://gist.github.com/milligramme/6173510.js"></script>


あと、入れ子の三項演算子とか処理できないので、 minify のされ方によって min.js は動かないことが多い

underscore.min.js はエラーになる

