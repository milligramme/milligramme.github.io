+++
title = "InDesign.jsxの非同期書き出し可能なのはpdfとidml"
date = "2014-10-30T00:00:00+09:00"
tags = ["extendscript", "indesign","pdf","idml"]
+++

非同期書き出しができるのはpdfとidmlだけ(CS5だと)

```js
#target "indesign-7.0"
var doc = app.documents[0];
var fmt;
var format = [
  'EPS_TYPE',
  'INCOPY',
  'INCOPY_CS_DOCUMENT',
  'INCOPY_CS2_STORY',
  'INCOPY_DOCUMENT',
  'INCOPY_MARKUP',
  'INDESIGN_INTERCHANGE',
  'INDESIGN_MARKUP',
  'INDESIGN_SNIPPET',
  'INTERACTIVE_PDF',
  'JPG',
  'PACKAGED_XFL',
  'PDF_TYPE',
  'PNG_FORMAT',
  'RTF',
  'SVG',
  'SVG_COMPRESSED',
  'SWF',
  'TAGGED_TEXT',
  'TEXT_TYPE',
  'XML',
  'HTML',
  'EPUB',
]

var export_file_test = function (doc) {
  for (var i=0, len=format.length; i < len ; i++) {
    // async
    try {
      fmt = ExportFormat[format[i]];
      doc.asynchronousExportFile(fmt, File("~/Desktop/test/Async-"+ format[i]));
      $.writeln('Asynchronous Done ' + format[i]);
    }
    catch(x_x){}
    // sync
    try {
      fmt = ExportFormat[format[i]];
      doc.exportFile(fmt, File("~/Desktop/test/" + format[i]));
      $.writeln('Done '+ format[i]);
    }
    catch(x_x){}
  };
}
$.writeln("_Document_");
export_file_test(doc);

$.writeln("\n_Story_");
doc.textFrames[0].texts[0].select();
export_file_test(doc.selection[0]);

// _Document_
// Done EPS_TYPE
// Asynchronous Done INDESIGN_MARKUP
// Done INDESIGN_MARKUP
// Done INTERACTIVE_PDF
// Done JPG
// Done PACKAGED_XFL
// Asynchronous Done PDF_TYPE
// Done PDF_TYPE
// Done SWF
// Done XML
//
// _Story_
// Done EPS_TYPE
// Done INCOPY_MARKUP
// Asynchronous Done INDESIGN_MARKUP
// Done INDESIGN_MARKUP
// Done INTERACTIVE_PDF
// Done JPG
// Done PACKAGED_XFL
// Asynchronous Done PDF_TYPE
// Done PDF_TYPE
// Done RTF
// Done SWF
// Done TAGGED_TEXT
// Done TEXT_TYPE
// Done XML
```