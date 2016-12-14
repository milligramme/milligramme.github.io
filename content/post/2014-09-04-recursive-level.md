+++
title = "InDesign.jsxで再帰レベル"
date = "2014-09-04T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesign.jsxで入れ子になったオブジェクトの一番親のときだけしたい処理があって、再帰レベルの取得をこんなので試したけど、もっといい方法あるだろうか

```js
#target "InDesign"
var recursive_level_test = function (obj, level) {
  var cb = [];
  for (var i=0, len=obj.length; i < len ; i++) {
    if (typeof obj[i] !== 'object') {
      cb.push(obj[i] + " is level:" + level + (function(){(level===1)?ret="=PARENT":ret=""; return ret})());
    } 
    else {
      cb.push(arguments.callee(obj[i], level + 1));
    }
  }
  return cb
}



var ary = [
  1,2,3,4,5, // こいつら
    [11,22,33,
      [111,222,333],
    44,55],
  6,7,8]; // こいつら

var result = recursive_level_test(ary, 1);
$.writeln(result.toSource());  
// ["1 is level:1=PARENT", "2 is level:1=PARENT", "3 is level:1=PARENT", "4 is level:1=PARENT", "5 is level:1=PARENT", ["11 is level:2", "22 is level:2", "33 is level:2", ["111 is level:3", "222 is level:3", "333 is level:3"], "44 is level:2", "55 is level:2"], "6 is level:1=PARENT", "7 is level:1=PARENT", "8 is level:1=PARENT"]
```
