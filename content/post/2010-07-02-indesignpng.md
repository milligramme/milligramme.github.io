+++
title = "PNG書き出し"
date = "2010-07-02T00:00:00+09:00"
tags = ["indesign"]

outdated = true
+++
InDesignの標準機能で png書き出しはないけど、スクリプトならできます。

ただし、jpegやeps、pdf書き出しと違い、なぜかExportOptionsがないので細かい設定ができません。ドキュメント上の原寸サイズで、72ppiの透過png（レイヤー生きなら）が書き出されるようです。

もうすこし高解像度にしたければ......（その方法は、ちょっと考えればわかると思います）。

pngを何に使うかといえば、○○○○絡みでInDesignの○○○○書き出しのオプションで画像形式が、まだjpegとgifしか選択できないので、透過png使ってみたいなーなんて。

あとInDesignのグラフィックツールで描画したオブジェクトが○○○○では無視されるんで、それの透過png書き出しとか......。

![/images/2010/09/765-png_export.png](/images/2010/09/765-png_export.png)

```js
//PNG書き出し
var sel = app.selection[0];
var file = new File("~/Desktop/pudding.png");
var feedback = exportPNG(sel, file);
alert(feedback);//boolean
/**
* exportPNG
* @param {Object} target Selected Object to export PNG
* @param {Object} file Export File Destination
*
* @returns {Boolean} true if success to export
*
* export 72ppi transparent png no options exist
*/
function exportPNG (target, file) {
  if(target.allGraphics.length == 1){
    var graphicObj = target.allGraphics[0];
    switch(graphicObj.constructor.name){
      case "PICT" :
      case "EPS" :
      case "WMF" :
      case "PDF" :
      case "Image" ://PSD/TIFF/JPEG/BMP
      case "ImportedPage" ://INDD
        try{
          target.exportFile(ExportFormat.PNG_FORMAT, file );
          return true;
        }catch(e){
          //$.writeln("saving was failed\r"+e);
          return false;
        }
        break;
      default : return false;
    }
  }
  else{
    switch(target.constructor.name){
      case "Polygon":
      case "Rectangle":
      case "Oval":
      case "Group":
      case "GraphicLine" :
      case "TextFrame":
      try{
        target.exportFile(ExportFormat.PNG_FORMAT, file );
        return true;
      }catch(e){
        //$.writeln("saving was failed\r"+e);
        return false;
      }
      break;
    default : return false;
    }
  }
}
```