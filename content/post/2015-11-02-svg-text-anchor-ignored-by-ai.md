+++
title = "IllustratorでSVGのtext-anchorが無視される"
date = "2015-11-02T00:00:00+09:00"
tags = ["svg", "illustrator", "extendscript"]
+++

```xml
<?xml version="1.0"?>
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" height="300">
  <line x1="150" y1="20" x2="150" y2="280" style="stroke-width:0.3; stroke:#900;"/>
  <text x="150" y="100" style="text-anchor:start; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">START</text>
  <text x="150" y="150" style="text-anchor:middle; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">MIDDLE</text>
  <text x="150" y="200" style="text-anchor:end; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">END</text>
</svg> 
```

手書きしたsvgを Illustrator で開くと `style="text-anchor:xxxx"` が無視される

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 17.1.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd" [
	<!ENTITY ns_extend "http://ns.adobe.com/Extensibility/1.0/">
	<!ENTITY ns_ai "http://ns.adobe.com/AdobeIllustrator/10.0/">
	<!ENTITY ns_graphs "http://ns.adobe.com/Graphs/1.0/">
	<!ENTITY ns_vars "http://ns.adobe.com/Variables/1.0/">
	<!ENTITY ns_imrep "http://ns.adobe.com/ImageReplacement/1.0/">
	<!ENTITY ns_sfw "http://ns.adobe.com/SaveForWeb/1.0/">
	<!ENTITY ns_custom "http://ns.adobe.com/GenericCustomNamespace/1.0/">
	<!ENTITY ns_adobe_xpath "http://ns.adobe.com/XPath/1.0/">
]>
<svg version="1.1" id="レイヤー_1" xmlns:x="&ns_extend;" xmlns:i="&ns_ai;" xmlns:graph="&ns_graphs;"
	 xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 300 300"
	 enable-background="new 0 0 300 300" xml:space="preserve">
<switch>
	<foreignObject requiredExtensions="&ns_ai;" x="0" y="0" width="1" height="1">
		<i:pgfRef  xlink:href="#adobe_illustrator_pgf">
		</i:pgfRef>
	</foreignObject>
	<g i:extraneous="self">
		<text transform="matrix(1 0 0 1 150 99.9999)" font-family="'ShinGoPro-Light-83pv-RKSJ-H'" font-size="7">START</text>
		<text transform="matrix(1 0 0 1 135.3156 149.9999)" font-family="'ShinGoPro-Light-83pv-RKSJ-H'" font-size="7">MIDDLE</text>
		<text transform="matrix(1 0 0 1 133.6287 199.9997)" font-family="'ShinGoPro-Light-83pv-RKSJ-H'" font-size="7">END</text>
	</g>
</switch>
<i:pgf  id="adobe_illustrator_pgf">
	<![CDATA[
...
	]]>
	<![CDATA[
...
	]]>
	<![CDATA[
...
	]]>
	<![CDATA[
...
	]]>
	<![CDATA[
...
	]]>
	<![CDATA[
...
	]]>
</i:pgf>
</svg>
```

センター揃え、右揃えのテキストがすべてアンカーポイントが x=150 から移動して左揃えになる

```xml
<text transform="matrix(1 0 0 1 150 99.9999)">START</text>
<text transform="matrix(1 0 0 1 135.3156 149.9999)">MIDDLE</text>
<text transform="matrix(1 0 0 1 133.6287 199.9997)">END</text>
```

のでアンカーを x=150 で左揃えに生成しておいて、テキストをid指定する。

Illustrator上での object.name になるのでid重複していてもok

```xml
<?xml version="1.0"?>
<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" height="300">
  <line x1="150" y1="20" x2="150" y2="280" style="stroke-width:0.3; stroke:#900;"/>
  <text id="start" x="150" y="100" style="text-anchor:start; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">START</text>
  <text id="middle" x="150" y="150" style="text-anchor:start; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">MIDDLE</text>
  <text id="end" x="150" y="200" style="text-anchor:start; stroke:none; fill:#000;" font-family="ShinGoPro-Light-83pv-RKSJ-H" font-size="7">END</text>  
  </g>
</svg> 
```

jsxで後処理すると、左揃え以外にもできる

```js
#target "Illustrator"

var doc = app.documents[0];

var tfs = doc.textFrames;
for (var i=0, len=tfs.length; i < len ; i++) {
  var textframe = tfs[i];
  if (textframe.name == "start") {
    textframe.paragraphs[0].justification = Justification.LEFT;
  }
  else if (textframe.name == "middle") {
    textframe.paragraphs[0].justification = Justification.CENTER;
  }
  else if (textframe.name == "end") {
    textframe.paragraphs[0].justification = Justification.RIGHT;
  }
  else {}
};
```
