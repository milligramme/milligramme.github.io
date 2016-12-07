+++
title = "InDesignでストーリー上の表組のjpeg書き出し"
date = "2014-09-03T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesignのjpeg書き出し、対象がフレームオブジェクトじゃないとページ単位になってしまう

テキストストーリー上の表組みだけを画像化したいときに、そのままじゃできないので仮テキストフレームをつくってその中に複製し書き出すようにしてみた

```js
var table_to_jpg = function (parent_character) {
  
  // 表が挿入されているcharacter
  var target = parent_character;
  var doc = app.documents[0];
  
  // 一時的テキストフレームに複製
  var tmp_textframe = doc.textFrames.add({geometricBounds:[0,0,10,10]});
  target.duplicate(LocationOptions.AT_END, tmp_textframe.parentStory.insertionPoints[0]);
  tmp_textframe.fit(FitOptions.FRAME_TO_CONTENT);
  
  // 書き出しパス
  var export_path = Folder.temp + '/table_' + (Math.random() * Math.pow(10,16)).toString(36) + '.jpg';
  
  // jpeg書き出しオプション
  // 毎回やることないので、外に出してもいい
  var jpeg_export_pref = app.jpegExportPreferences;
  
  jpeg_export_pref.exportingSpread    = true;
  jpeg_export_pref.exportResolution   = 150;
  jpeg_export_pref.jpegColorSpace     = JpegColorSpaceEnum.RGB;
  jpeg_export_pref.jpegRenderingStyle = JPEGOptionsFormat.BASELINE_ENCODING
  jpeg_export_pref.jpegExportRange    = ExportRangeOrAllPages.EXPORT_RANGE;
  jpeg_export_pref.jpegQuality        = JPEGOptionsQuality.HIGH;
  
  tmp_textframe.exportFile(ExportFormat.JPG, export_path);
    
  // 一時的テキストフレームを削除
  tmp_textframe.remove();

  return export_path
}
```

### 参考にした

[Random alpha-numeric string in JavaScript? - Stack Overflow](http://stackoverflow.com/questions/10726909/random-alpha-numeric-string-in-javascript)