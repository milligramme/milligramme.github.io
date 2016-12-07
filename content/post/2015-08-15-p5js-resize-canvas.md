+++
title = "p5jsでcanvasサイズの可変"
date = "2015-08-15T00:00:00+09:00"
tags = ["processing", "p5js"]
+++

### canvasサイズの可変

```js
function setup() {
  createCanvas(windowWidth, windowHeight);
}

function draw() {
  background(0, 100, 200);
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
```


[Beyond the canvas · processing/p5\.js Wiki](https://github.com/processing/p5.js/wiki/beyond-the-canvas)