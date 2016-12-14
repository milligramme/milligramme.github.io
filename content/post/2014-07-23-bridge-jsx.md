+++
title = "Bridge.jsxでPhotoshop.jsx"
date = "2014-07-23T00:00:00+09:00"
tags = ["extendscript", "bridge"]
+++

Photoshop.jsx の流れで Bridge.jsx を少しいじってみた

```js
#target 'bridge'
var canvas = new BitmapData(new File("~/Desktop/test.psd"));
$.writeln(canvas.reflect.methods);
// clone,dispose,exportTo,getPixel,getPixel32,loadFromJpegStream,loadFromPngStream,resize,rotate,setPixel,setPixel32

$.writeln(canvas.getPixel(0,0)); // => 4013377
$.writeln(canvas.getPixel(0,0).toString(16)); // => 3d3d41

canvas.resize([16,16],"bicubic");

// File書き出ししないと変更が反映されない
canvas.exportTo(new File("~/Desktop/test.jpg"), 100); // jpegでしか書き出しされない
canvas.exportTo(new File("~/Desktop/test.png"), 100); // 拡張子pngなjpegが書き出される
```

そんなに弄れないので、内容によっては [sips](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/sips.1.html) や  [layervault/psd.rb](https://github.com/layervault/psd.rb) のほうがよさそうという印象

### 参考にした

[ASCII.jp：Photoshopを超えた!?BridgeとJSで作る画像フィルター (1/6)｜古籏一浩のJavaScriptラボ](http://ascii.jp/elem/000/000/500/500608/)