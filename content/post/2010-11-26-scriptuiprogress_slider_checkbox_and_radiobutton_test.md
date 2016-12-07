+++
title = "Progress Slider, CheckBox and RadioButton Test"
date = "2010-11-26T00:00:00+09:00"
tags = ["scriptui"]
+++

プログレスバーのかわりにスライダーやチェックボックス、ラジオボタンで経過表現ができるかという試み。

チェックボックスとラジオボタンはアナログシーケンサーのインジケーターのようにカクカクと、意外にスライダーはそれなりにスムーズに進行していきます。

ただしフローティングウィンドウにカーソルを近づけると、再描写の繰り返しがつらいのか挙動が少し怪しくなったりします。

![/images/2010/11/progress_slider.png](/images/2010/11/progress_slider.png)

![/images/2010/11/progress_checkbox.png](/images/2010/11/progress_checkbox.png)

![/images/2010/11/progress_radiobutton.png](/images/2010/11/progress_radiobutton.png)

### 結論

：できなくはないけどあえてやることもない。

```js
/**
 * progress slider, check box, radio button
 * 
 * 2010-11-25 mg
 */

for (var i=0, iL = 1000 ; i < iL; i++) {
  var max_value = iL;
  var win_width = 300;
  var now = i;
  var sld_pg = progress_slider (max_value, win_width, now);
  sld_pg.show();
};
sld_pg.close();

for (var i=0, iL = 100; i < iL; i++) {
  box_num = 12;
  var now = Math.floor(i/iL * box_num);
  var chb_pg = progress_checkbox (box_num, now);
  chb_pg.show();
};
chb_pg.close();

for (var i=0, iL = 100; i < iL; i++) {
  rdb_num = 10;
  var now = Math.floor(i/iL * rdb_num);
  var rdb_pg = progress_radiobutton (rdb_num, now);
  rdb_pg.show();
};
rdb_pg.close();

function progress_slider (max_value, win_width, now) {
  var pgb = new Window('window',"progress slider", undefined);
  var sld = pgb.add('slider',[0, 0, win_width, 23], now, 0, max_value);
  var tex = pgb.add('statictext', [0, 0, win_width, 23], now);
  sld.value = now;
  tex.text = now + " / " + max_value;
  return pgb;
}

function progress_checkbox (box_num, now) {
  var pgc = new Window('window', "progress checkbox", undefined);
  pgc.orientation = 'row';
  var ch_arr = [];
  for (var i=0; i < box_num; i++) {
    ch_arr.push(pgc.add('checkbox',[0,0,23,23],""));
  };
  ch_arr[now].value = true;
  return pgc
}

function progress_radiobutton (rdb_num, now) {
  var pgr = new Window('window', "progress radiobutton", undefined);
  pgr.orientation = 'row';
  var rd_arr = [];
  for (var i=0; i < rdb_num; i++) {
    rd_arr.push(pgr.add('radiobutton',[0,0,23,23],""));
  };
  rd_arr[now].value = true;
  return pgr
}
```