+++
title = "ExtendScriptでjsonの読み込みをincludeで行う"
date = "2016-09-07T12:44:00+09:00"
tags = ["extendscript", "json"]

outdated = false 
+++

json < javascript

before

```js
//@include "/path/to/json2.js"
var json;
var json_file = File("/path/to/test.json");
if (json_file.open("r")) {
  json = JSON.parse(json_file.read())
  json_file.close();
}
json.toSource();
```

after

```
var json = 
//@include "/path/to/test.json"
json.toSource();
```
