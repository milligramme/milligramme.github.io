+++
title = "jsxでrubyの _FILE_==$0みたいなのをやる"
date = "2013-05-23T00:00:00+09:00"
tags = ["extendscript"]
+++

[github.com/milligramme/jsx_run_and_lib][milligramme/jsx_run_and_lib] の転載

ruby でいう `require` されたときは実行されないけど単体実行だと実行される

```ruby
def foo(x)
  # xxx
end

if __FILE__ == $0
  # メソッドのテストとか
  # foo x
end
```

をjsxでもやりたい。

`$.stack` と `$.fileName` を比較して、`#include` されたときは実行しない、単体実行したときは実行する分岐を実現する書き方。  ライブラリのおしりに記述して単体でテストするなどに便利

```js
var foo = function(x){
  // xxx
}
if ($.stack.replace(/[\[\]\n]/g,"") == new File($.fileName).name) {
  // 関数テストとか
  // foo(x);
}
```

<!-- リンクを変数っぽくかける -->
[milligramme/jsx_run_and_lib]: https://github.com/milligramme/jsx_run_and_lib
