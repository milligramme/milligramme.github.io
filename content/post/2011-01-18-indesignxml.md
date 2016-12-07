+++
title = "スタイル属性付XMLでオブジェクトスタイルの読み込みは未サポート"
date = "2011-01-18T00:00:00+09:00"
tags = ["indesign", "xml"]
+++
前々回、XMLにInDesignの段落スタイルと文字スタイルを記述して読み込む、「 [[InDesign]スタイル属性付XMLをInDesignに読み込む](http://www.milligramme.cc/wp/archives/3424) 」というのをやってみて、同様にオブジェクトスタイルも

**<image aid:ostyle="オブジェクトスタイル名"></image>**

でできないものかと試してみましたが、残念ながらオブジェクトスタイルの属性読み込みは実装されていないみたいです。 そういえば、 @osimajp さんもそんなデモまではやってませんでした。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<Root xmlns:aid5="http://ns.adobe.com/AdobeInDesign/5.0/" xmlns:aid="http://ns.adobe.com/AdobeInDesign/4.0/">
<body>
    <title><p aid:pstyle="title">THIS IS TITLE</p></title>
    <tom><image aid:ostyle="cat" href="file:///cat.jpg"></image></tom>
    <jerry><image aid:ostyle="mice" href="file:///mouse.jpg"></image></jerry>    
</body>
</Root>
```

GUIのタグパネルの「タグをスタイルにマップ」でも、オブジェクトスタイルはポップアップされません。
![/images/2011/01/tag_to_style.png](/images/2011/01/tag_to_style.png)

XMLImportMap の mappedStyle プロパティも CharacterStyle と ParagraphStyle にしか対応してない。

Googleで「aid:ostyle」と検索してみても、アドベフォーラムの製品の要望（Feature Request）のディスカッションページくらいしか引っかからないし、こういう使い方は需要がないのだろうか？
とりあえず決め撃ちだけど、image タグにあてた aid:ostyleのオブジェクトスタイルをコンテナフレームに適用してみる。

```js

var doc = app.documents[0];
var xml_root = doc.xmlElements[0]; //set root
var img = xml_root.xmlElements[0].xmlElements.itemByName("image"); // get image tag
var obj_style_name = img.xmlAttributes.item('aid:ostyle').value; // get ostyle value
img.images[0].parent.appliedObjectStyle = doc.objectStyles.item(obj_style_name); // apply ostyle to rectangle
```

こんな感じで。本来なら再帰処理で階層をたどっていくのでしょう。何となく見えてきた。