+++
title = "InDesignのデータ結合をjsxで行う"
date = "2016-10-21T19:41:00+09:00"
tags = ["indesign", "datamerge", "extendscript"]

outdated = false
+++

InDesignの既存のデータ結合の後に、jsxで何か後処理する必要があるときに
GUI -> jsxでなくjsxの実行一発でやりたかったので調べた。

テンプレのinddでデータ結合の手続きが済んでいることが前提条件。

```js
//@target indesign
main("/PATH/TO/TEMPLATE.indd", "/PATH/TO/UTF16LE-BOM_SOURCE.csv");

function main (template_path, csv_path) {
  var template_doc = app.open(File(template_path));
  template_doc.dataMergeProperties.selectDataSource(File(csv_path));
  template_doc.dataMergeProperties.updateDataSource();
  template_doc.dataMergeProperties.mergeRecords();
  
  var merged_doc = app.documents[0];
  merged_doc = after_do(merged_doc);
  merged_doc.save(File(template_path.toString().replace(/\.indd$/,"-merged.indd")));
  template_doc.close(SaveOptions.NO);
  merged_doc.close(SaveOptions.NO);
}

var after_do = function (doc) {
  
  // DO SOMETHING
  // DO SOMETHING
  // DO SOMETHING
  // DO SOMETHING
  
  return doc
}
```