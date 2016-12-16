+++
title = "ScriptUIでパネルの裏に隠れるプログレスバーの回避策"
date = "2012-01-06T00:00:00+09:00"
tags = ["scriptui", "extendscript"]
+++

長い処理の進捗確認のためのプログレスバーを使っていて、うっかりInDesignなりIllustratorのパネルが出ていると、そのパネルの裏にプログレスバーが隠れてしまって残念だったので、回避する方法考えてみた。

ScriptUIのWindowタイプを "palette"にするとよいみたい。（AdobeのプログレスバーのサンプルスクリプトはWindowタイプが "window"になってる）

### Window type = "palette"

[![](/images/2012/01/progress_plt.png "progress_plt")](/images/2012/01/progress_plt.png)

パネルの上になる

### Window type = "window"

[![](/images/2012/01/progress_win.png "progress_win")](/images/2012/01/progress_win.png)

隠れる

### Window type = "dialog"（おまけ）

[![](/images/2012/01/progress_dlg.png "progress_dlg")](/images/2012/01/progress_dlg.png)

```js
// panels and progressbar z-order-test
#targetengine 'session'
var u;
var type = ["palette", "window", "dialog"];
for (var i=0; i < type.length; i++) {
  var w = new Window(type[i], "z-order-test", u);
  w.orientation = 'column';
  w.preferredSize = [320,100];
  w.alignChildren = ['fill', 'fill'];

  w.add('statictext', u, w.type);
  p = w.add('progressbar', u, 0, 100);
  if (type[i]=='dialog') {
    var btn = w.add('button', u, "Close", {name: "ok"});
    btn.onClick = function() {w.close();};
  };
  p.value = 1;
  w.show();
  $.sleep(5000);
  w.close();
};
```