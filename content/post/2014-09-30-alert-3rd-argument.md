+++
title = "jsxのalertのアイコンをエラー用にする"
date = "2014-09-30T00:00:00+09:00"
tags = ["extendscript"]
+++

`alert` の第三引数にオブジェクトで errorIcon をtrue指定するとアイコンが若干エラー用になる

```js
alert("アラート\nアラートです");

alert("エラー\nエラーです", "windowsだとタイトルになるけどMacだと無視される", {errorIcon:true});
```

![2014 09 30 Alert Icon](/images/2014-09-30-alert-icon.png)

![2014 09 30 Error Icon](/images/2014-09-30-error-icon.png)

これ、 [milligramme/scriptui_scrollable_alert](https://github.com/milligramme/scriptui_scrollable_alert) にも実装したい

