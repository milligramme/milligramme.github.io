+++
title = "オーバーフローチェック2種比較"
date = "2011-09-06T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
InDesignでドキュメントがオーバーフローしてるかしてないかのプリフライト的チェックするのにアプローチの違う2つのもので比べてみた。長体処理とかはしません。

- ストーリーのオーバーフローをチェックしてしてたら、テキストフレーム（textContainers）を舐める
- スプレッドごとにテキストフレームを舐めていく


チェックするドキュメントの内容にもよるんだろうけど、ストーリーで攻めた方が速い気がする。

ベンチマークに `$.hiresTimer` を使っているのでCS4以降ですが、

`new Date().getTime()` でも全然問題ない（単に使いたかっただけ）

overflow_check_via_story() の方は TextContainer を使っているので CS3以降でないと使えません。

```js
var doc = app.documents[0];
a = $.hiresTimer // ベースの時間
overflow_check_via_story (doc);
b = $.hiresTimer
overflow_check_via_spread (doc);
c = $.hiresTimer
$.writeln(b); // 差分以下同
$.writeln(c);

function overflow_check_via_story (doc) {
  var story_obj = doc.stories;
  var error_overflow = [];

  for (var sti=0, stiL=story_obj.length; sti < stiL ; sti++) {
    if (story_obj[sti].overflows) {
      var tx_container_obj = story_obj[sti].textContainers;
      for (var tci=0, tciL=tx_container_obj.length; tci < tciL ; tci++) {
        if (tx_container_obj[tci].overflows) {
          error_overflow.push(tx_container_obj[tci].parent);
        };
      };
    }
  };
  return error_overflow
}

function overflow_check_via_spread (doc) {
  var spread_obj = doc.spreads;
  var error_overflow2 = [];

  for (var spi=0, spiL=spread_obj.length; spi < spiL ; spi++) {
    var tf_obj = spread_obj[spi].textFrames;
    for (var tci=0, tciL=tf_obj.length; tci < tciL ; tci++) {
      if (tf_obj[tci].overflows) {
        error_overflow2.push(tf_obj[tci].parent);
      };
    };
  }
  return error_overflow2
};
```
