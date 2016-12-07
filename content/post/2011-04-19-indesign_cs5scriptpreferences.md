+++
title = "ScriptPreferencesによる一時的単位変換"
date = "2011-04-19T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

InDesign CS5から ScriptPreferencesでドキュメントの単位系を変えることなくスクリプト処理に対して指定の単位系に換算してくれるようなのでメモ。

InDesign に限らず ExtendScript では内部的にポイントで処理しているので、単位系に級歯メートルをつかっているとどうしても誤差が生じる。（たまにポイントでしか値を返さないプロパティーもある）なので一時的にドM系ドキュメントでは単位系を一度ポイントへ変換して、バックアップから単位系を戻すような処理をしている（ことが多い）。

CS4まで最大5種類の単位系をいじる

![/images/2011/04/document_unit.png](/images/2011/04/document_unit.png)

バックアップして戻す

```js
function before_after_unit (func, arg1, arg2) {
  var doc = app.documents[0];
  var bk_lin  = doc.viewPreferences.lineMeasurementUnits;
  var bk_typo = doc.viewPreferences.typographicMeasurementUnits;
  var bk_txt  = doc.viewPreferences.textSizeMeasurementUnits;
  var bk_hori = doc.viewPreferences.horizontalMeasurementUnits;
  var bk_vert = doc.viewPreferences.verticalMeasurementUnits;

  with (doc.viewPreferences){
    lineMeasurementUnits        = MeasurementUnits.POINTS;
    typographicMeasurementUnits = MeasurementUnits.POINTS;
    textSizeMeasurementUnits    = MeasurementUnits.POINTS;
    horizontalMeasurementUnits  = MeasurementUnits.POINTS;
    verticalMeasurementUnits    = MeasurementUnits.POINTS;
  }

  func(arg1, arg2); // 実際の処理

  with (doc.viewPreferences){
    lineMeasurementUnits        = bk_lin;
    typographicMeasurementUnits = bk_typo;
    textSizeMeasurementUnits    = bk_txt;
    horizontalMeasurementUnits  = bk_hori; 
    verticalMeasurementUnits    = bk_vert; 
  }  
}
```

などと単位変換をかましてました。

CS5からだと

```js

if (app.version.split('.')[0]-0 >= 7) {
  app.scriptPreferences.measurementUnit = MeasurementUnits.POINTS;
  main();
  app.scriptPreferences.measurementUnit = AutoEnum.AUTO_VALUE;
  // => AUTO_VALUEドキュメントの元の単位系に戻ってる
};
```
細かく設定はできませんが、すっきりとこんな記述で済みます。

注意点は、単位変換が有効なのがスクリプト単位でなく、アプリケーション全体で共有するので、AUTO_VALUEに戻してあげないと意図しない単位系のまま別の処理をしてしまうことがあります。

簡単なスクリプトでチェックしました。

```js
#target "InDesign"
if (app.version.split('.')[0]-0 >= 7) {
  app.scriptPreferences.measurementUnit = MeasurementUnits.POINTS;
  main();  
  $.writeln("\nNOW: " + app.scriptPreferences.measurementUnit);
  // app.scriptPreferences.measurementUnit = AutoEnum.AUTO_VALUE
};

function main () {
  var U = ['MILLIMETERS', 'POINTS', 'Q', 'HA', 'INCHES'];
  for (var i=0; i < U.length; i++) {
    switch_unit(U[i]);

    var doc = app.documents.length === 0 ? app.documents.add() : app.documents[0];

    var doc_w = doc.documentPreferences.pageWidth;
    var doc_h = doc.documentPreferences.pageHeight;

    var tfram = doc.textFrames.length === 0 ? doc.textFrames.add({
      geometricBounds : ['10mm','10mm','90mm','120mm'],
      fillColor : "Cyan",
      strokeColor : "Black",
      strokeWeight : '1mm'
    })
    : doc.textFrames[0];
    tfram.contents = TextFrameContents.placeholderText;

    $.writeln("\n>"+U[i]);
    $.writeln("Doc Width:"+doc_w);
    $.writeln("Doc Height:"+doc_h);
    $.writeln("TF Stroke Weight:"+tfram.strokeWeight);
    $.writeln("TF Bounds:"+tfram.geometricBounds);
    $.writeln("TF Point Size:"+tfram.parentStory.pointSize);

  };

}
function switch_unit (n) {
  MU = MeasurementUnits[n];
  app.scriptPreferences.measurementUnit = MU;
}
```


結果

```
>MILLIMETERS
Doc Width:209.999999999936
Doc Height:296.999999999461
TF Stroke Weight:1
TF Bounds:10.0000000000003,9.99999999999925,90.0000000000003,119.999999999999
TF Point Size:3.25

>POINTS
Doc Width:595.275590551
Doc Height:841.889763778
TF Stroke Weight:2.83464566929134
TF Bounds:28.3464566929143,28.3464566929113,255.118110236221,340.157480314959
TF Point Size:9.21259842519685

>Q
Doc Width:839.999999999745
Doc Height:1187.99999999784
TF Stroke Weight:4
TF Bounds:40.0000000000013,39.999999999997,360.000000000001,479.999999999997
TF Point Size:13

>HA
Doc Width:839.999999999745
Doc Height:1187.99999999784
TF Stroke Weight:4
TF Bounds:40.0000000000013,39.999999999997,360.000000000001,479.999999999997
TF Point Size:13

>INCHES
Doc Width:8.26771653543056
Doc Height:11.6929133858056
TF Stroke Weight:0.03937007874016
TF Bounds:0.39370078740159,0.39370078740155,3.54330708661419,4.72440944881887
TF Point Size:0.12795275590551

NOW: 2053729891 /// 単位系はインチになってます。
```

既に誤差が気持ち悪いです。
別なスクリプトをそのまま実行してみます。

```js
$.writeln(app.scriptPreferences.measurementUnit); // ==> INCHES
app.scriptPreferences.measurementUnit = AutoEnum.AUTO_VALUE;

$.writeln(app.documents[0].documentPreferences.pageWidth); 
```

単位系はインチのままです。AUTO_VALUEで初期化しないと、このスクリプトはインチ単位系で処理をしてしまいます。