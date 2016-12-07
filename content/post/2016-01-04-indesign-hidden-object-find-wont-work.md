+++
title = "InDesign.jsxで隠したドキュメントはオブジェクト検索ができない"
date = "2016-01-04T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

オブジェクト検索は hiddenなdocではむり

[findObject\(\) inside hidden files \| Adobe Community](https://forums.adobe.com/thread/2054527)

```js
var find_object_in_doc = function (doc_shown) {
  var doc = app.documents.add(doc_shown);
  doc.rectangles.add();
  doc.rectangles.add();
  doc.rectangles.add();
  doc.rectangles.add();

  var find_object_obj = {
    strokeWeight: "1pt",
    strokeType: "$ID/Solid",
    strokeColor: "$ID/Black",
    fillColor: "$ID/[None]"
  };
  doc.rectangles.everyItem().properties = find_object_obj;

  var match = object_find(doc, find_object_obj);
  
  $.writeln("4 rectangles will be found");
  $.writeln(match.length);
  
}

with(app.findChangeObjectOptions){
  includeFootnotes            = true;
  includeHiddenLayers         = true;
  includeLockedLayersForFind  = true;
  includeLockedStoriesForFind = true;
  includeMasterPages          = true;
}


function object_find(target_obj, find_object_obj){
  app.findObjectPreferences = NothingEnum.nothing;
  app.findObjectPreferences.properties = find_object_obj;
  var result = target_obj.findObject();
  app.findObjectPreferences = NothingEnum.nothing;
  return result;
}

// shown doc
find_object_in_doc(true); // 4つみつかる
app.documents.everyItem().close(SaveOptions.NO);

// hidden doc
find_object_in_doc(false); // マッチしない

// result
// => 4 rectangles will be found!
// => 4
// => 4 rectangles will be found!
// => 0
```

---

ちなみにテキスト検索、GREP検索はできる

```js
var target_obj = app.documents.add(false);
var t = target_obj.textFrames.add({geometricBounds:[0,0,100,120]});
t.parentStory.contents = "find find find FOUND";

var find_txt_obj = {
  findWhat : "find"
};

with(app.findChangeTextOptions){
  caseSensitive               = false;
  includeFootnotes            = false;
  includeHiddenLayers         = false;
  includeLockedLayersForFind  = false;
  includeLockedStoriesForFind = false;
  includeMasterPages          = false;
  kanaSensitive               = true;
  wholeWord                   = false;
  widthSensitive              = true;
}

var match = text_find(target_obj, find_txt_obj);
$.writeln(match.length); // should 3

function text_find(target_obj, find_txt_obj){
  app.findTextPreferences = NothingEnum.nothing;
  app.findTextPreferences.properties = find_txt_obj;
  var result = target_obj.findText();
  app.findTextPreferences = NothingEnum.nothing;
  return result;
}

var find_grep_obj = {
  findWhat : "[A-Z]"
};

with(app.findChangeGrepOptions){
  includeFootnotes            = false;
  includeHiddenLayers         = false;
  includeLockedLayersForFind  = false;
  includeLockedStoriesForFind = false;
  includeMasterPages          = false;
  kanaSensitive               = true;
  widthSensitive              = true;
}

var match = grep_find(target_obj, find_grep_obj);
$.writeln(match.length); // should 5

function grep_find(target_obj, find_grep_obj){
  app.findGrepPreferences = NothingEnum.nothing;
  app.findGrepPreferences.properties = find_grep_obj;
  var result = target_obj.findGrep();
  app.findGrepPreferences = NothingEnum.nothing;
  return result;
}

// result
// => 3
// => 5
```