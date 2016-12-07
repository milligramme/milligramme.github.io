+++
title = "新しいタブで Vomnibar"
date = "2014-03-13T00:00:00+09:00"
tags = ["chrome", "vimium"]
+++

GoogleChrome.app + [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb) つかってて、新規タブが Chrome 内蔵ページになってしまい Vomnibar が出てこないがつらいので調べたら、現状では新規タブを別の url に変更出来ないっぽい

[New Tab Redirect](https://chrome.google.com/webstore/detail/new-tab-redirect/icpgjfneehieebagbmdbhnlpiopdcmna) という拡張を使うと変更できるのでインストールし、sinatra で簡単なページをつくって、

[Pow](http://pow.cx/) で起動させておいてみた

http://new-tab.dev/ を New Tab Redirect で新規タブに設定しておくと

![2014 03 13 For Vomnibar](/images/2014-03-13_for_vomnibar.png)