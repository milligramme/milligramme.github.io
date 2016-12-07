+++
title = "CS3ではボタン幅を69px以下にできない"
date = "2010-12-01T00:00:00+09:00"
tags = ["scriptui", "extendscript"]

outdated = true
+++
CS3用にUI作っていて、小さなボタンの幅がコントロールができなかったのでメモ。

以前に Macでは23pxからボタンの形状がアクアから四角ベタに変わるというテストをしたスクリプトをちょっと改造してみました。

```js
/**
 * button width change test
 */
var dlg = new Window('dialog', "threshold of button width" , undefined);
dlg.panel = dlg.add('panel');
dlg.panel.margins = 0;
dlg.panel.spacing = 0;
for (var i=0; i < 100; i+=1) {
  if (i%5 === 0) {
    var grp = dlg.panel.add('group');
  };
  grp.add('button', [0, 0, 23+i, 22], "w" + (23+i) + "px" );
};
dlg.show();
```

CS3だとあるwidthまでは指定が無効になる

![/images/2010/12/result_button_cs3.png](/images/2010/12/result_button_cs3.png)

CS4だと小さいwidthにも対応してる

![/images/2010/12/result_button_cs4.png](/images/2010/12/result_button_cs4.png)

ボタンを無駄につくり過ぎたが、変わり目付近をPhotoshopの差の絶対値で比較してみると、CS3でのボタンのwidthの最小値が70pxということがわかります。

「兄さん、三倍じゃ駄目っすよ」と覚えるといいです。

![/images/2010/12/compare.png](/images/2010/12/compare.png)