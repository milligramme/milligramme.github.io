+++
title = "InDesign.jsxでdate-util.jsを使う"
date = "2016-06-16T10:25:00+09:00"
tags = ["nodejs", "extendscript"]
+++

[JerrySievert/date\-utils: Date Pollyfills for Node\.js and Browser](https://github.com/JerrySievert/date-utils)

適当な場所に [date\-utils/date\-utils\.js](https://github.com/JerrySievert/date-utils/blob/master/lib/date-utils.js) を配置して実行。

```js
#include "/path/to/date-utils.js"
#target "indesign"

var dt = new Date();
var datetime = dt.toFormat("YYYY-MM-DD-HH24-MI-SS");
$.writeln(datetime); // => 2016-06-16-10-24-41
```

minifyされた `date-utils.min.js` の方は案の定エラーになる。