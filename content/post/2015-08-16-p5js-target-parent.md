+++
title = "p5jsでcanvasのターゲットの指定したい"
date = "2015-08-16T00:00:00+09:00"
tags = ["processing", "p5js"]
+++

特に指定しないと scriptタグのあるところに canvasタグが生成されてしまう

### canvasのターゲットの指定

```js
var canvas;
function setup() {
  canvas = createCanvas(windowWidth, windowHeight);
  
  canval.parent('shape');
}
```

とすると id="shape" の要素下に `<canvas id="defaultCanvas" ...>` が描画される



[Beyond the canvas · processing/p5\.js Wiki](https://github.com/processing/p5.js/wiki/beyond-the-canvas)