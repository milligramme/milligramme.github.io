+++
title = "InDesign.jsxでinsertLabelのKeyを忘れてしまったら"
date = "2016-05-20T12:10:00+09:00"
tags = ["indesign", "extendscript", "idml"]
+++


```js
obj.label = "通常ラベル";

obj.insertLabel("great", "すばらしいラベル");
obj.extractLabel("great"); // => "すばらしいラベル"
```

`insertLabel` したkeyを取得するプロパティがないので、うっかりキーを忘れると `extractLabel` できない。

idmlにして Spreads/Spread_xxx.xml などの KeyValuePair をさぐるしかないのか?

```xml
<Label>
	<KeyValuePair Key="great" Value="すばらしいラベル"/>
	<KeyValuePair Key="Label" Value="通常ラベル！！"/>
	<KeyValuePair Key="label" Value="これも通常ラベル？"/>
</Label>
```


## おまけ

```js
obj.insertLabel("label", "これも通常ラベル？");

obj.label;// => 通常ラベル
obj.extractLabel("label"); // => これも通常ラベル？
obj.extractLabel("Label"); // => 通常ラベル

obj.insertLabel("label", "通常ラベル！！");
obj.label; // => 通常ラベル！！
```



