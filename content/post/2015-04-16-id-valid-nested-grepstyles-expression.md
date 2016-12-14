+++
title = "InDesignの正規表現スタイルの構文チェック"
date = "2015-04-16T00:00:00+09:00"
tags = ["indesign", "extendscript", "regexp"]
+++

InDesignの正規表現スタイルは、正規表現式に構文が間違った式を書かれていてもエラーにならないで、単に機能しないだけ。

ただのStringである式をRegExpオブジェクトにしてみて、おかしいのがないか検査するスクリプト。

<script src="https://gist.github.com/milligramme/c267bec20ee0e995adcb.js"></script>

