+++
title = "InDesignのlabelとinsertLabelのlabel"
date = "2014-02-12T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignで obj.insertLabel(key, val) で "label" をキー指定するのと、 obj.label プロパティでは別物が入る

```js
var sel = app.selection[0];

sel.insertLabel("label", "foo");
sel.label = "bar";

$.writeln(sel.extractLabel("label")); //=> "foo"
$.writeln(sel.label);                 //=> "bar"
```