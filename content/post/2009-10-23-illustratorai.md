+++
title = "レガシィなAiを変換（フォントとか）"
date = "2009-10-23T00:00:00+09:00"
tags = ["illustrator", "font", "extendscript"]
+++
ある媒体でやっとこQxp3からInDesign CS3に変換するのに伴って、Illustrator ver.8データ図版がいっぱいあるから変換しといて〜とページごとのデータをごっそりもらったので、とりあえずの変換用にJavaScriptで書いてみた。

変換時のポイントはver.10〜CSでのテキスト更新とocf,cid〜otfへのフォントの違い。

昔のデータだと拡張子無しとか禁止文字とかが多いんですよね。

Photoshop.eps書類も混じってたりするので拡張子でフィルターをかけて処理できない。

クリエータコードとか飛んでたりもするとも〜も〜。

しかたがないので該当するIllustrator書類を開いた状態で実行することにしました。

### 仕様

古いバージョンのIllustrator書類を「フォントを更新せず」に開いて実行。

Morisawa OTF, CIDを同名のOpenType Fontに置換、カーニング="自動"、トラッキング="0"に変更、ファイル名は"○○○○_.eps"でアンダーバー付きにしてCS3 eps再保存。

元ファイルはそのまま保存しないで閉じます。

### 既知の問題点

フォント情報をもった孤立点があるとフォント置換しません。

あと、フォント情報をもった空のパステキストに色がついた線オブジェクト（グラフィックを文字ツールでクリックしちゃったみたいな）も同様に置換しません。

そんな時はあとで削除なりアウトライン化してあげる必要があります。

（検出するスキルがなかった......）EPS保存自体あれなんですが、いまなおプレビューをあえて意図して「Macintosh（8-bitカラー）」にしている人っているのだろうか。

```js

/*
古いIllustrator書類を更新
"replace regacy ai"
使い方：
古いバージョンのIllustrator書類を「フォントを更新せず」に開いて実行。
Morisawa OTF, CIDを同名のOpenType Fontに置換、
カーニング="自動"、トラッキング="0"に変更、
ファイル名は"○○○○_.eps"でアンダーバー付きにしてCS3 eps再保存。
元ファイルはそのまま保存しないで閉じます。
動作確認：OS10.4.11 Illustrator CS3
milligramme
www.milligramme.cc
*/
//Aiにおまじないは必要か
app.userInteractionLevel = UserInteractionLevel.DISPLAYALERTS;
// var start = new Date().getTime();
for(var d = app.documents.length; d > 0; d--){
  var docObj = app.activeDocument;
  //フォントを更新する
  docObj.legacyTextItems.convertToNative();
  //レイヤーのロック解除
  var layObj = docObj.layers;
  for(var k = 0; k < layObj.length; k++){
    layObj[k].locked = false;
  }
  //まずはテキストフレーム
  var tfObj = docObj.textFrames;
  for(var i = 0; i < tfObj.length; i++){
  tfObj[i].locked = false;
    var trgText = tfObj[i].textRange.characters;
    for(var j = 0; j < trgText.length; j++){
      var orgFont = trgText[j].characterAttributes.textFont;
      //モリサワOCF/CIDをOTF Proに置換、PS名で追加削除する。
      switch(orgFont.name){
        case "Ryumin-Light" : repFont = app.textFonts.getByName ("RyuminPro-Light"); break;
        case "Ryumin-regular" : repFont = app.textFonts.getByName ("RyuminPro-Regular"); break;
        case "Ryumin-Medium" : repFont = app.textFonts.getByName ("RyuminPro-Medium"); break;
        case "Ryumin-Bold" : repFont = app.textFonts.getByName ("RyuminPro-Bold"); break;
        case "Ryumin-heavy" : repFont = app.textFonts.getByName ("RyuminPro-Heavy"); break;
        case "Ryumin-Ultra" : repFont = app.textFonts.getByName ("RyuminPro-Ultra"); break;
        case "GothicBBB-Medium" : repFont = app.textFonts.getByName ("GothicBBBPro-Medium"); break;
        case "FutoMinA101-Bold" : repFont = app.textFonts.getByName ("FutoMinA101Pro-Bold"); break;
        case "FutoGoB101-Bold" : repFont = app.textFonts.getByName ("FutoGoB101Pro-Bold"); break;
        case "MidashiMin-MA31" : repFont = app.textFonts.getByName ("MidashiMinPro-MA31"); break;
        case "MidashiGo-MB31" : repFont = app.textFonts.getByName ("MidashiGoPro-MB31"); break;
        case "ShinGo-Light" : repFont = app.textFonts.getByName ("ShinGoPro-Light"); break;
        case "ShinGo-regular" : repFont = app.textFonts.getByName ("ShinGoPro-Regular"); break;
        case "ShinGo-Medium" : repFont = app.textFonts.getByName ("ShinGoPro-Medium"); break;
        case "ShinGo-Bold" : repFont = app.textFonts.getByName ("ShinGoPro-Bold"); break;
        case "ShinGo-Ultra" : repFont = app.textFonts.getByName ("ShinGoPro-Ultra"); break;
        case "GothicMB101-Bold" : repFont = app.textFonts.getByName ("GothicMB101Pro-Bold"); break;
        case "GothicMB101-hea" : repFont = app.textFonts.getByName ("GothicMB101Pro-Heavy"); break;
        case "GothicMB101-Ult" : repFont = app.textFonts.getByName ("GothicMB101Pro-Ultra"); break;
        case "Jun101-Light" : repFont = app.textFonts.getByName ("Jun101Pro-Light"); break;
        //case "Jun201-regular" : repFont = app.textFonts.getByName ("Jun201Pro-regular"); break;
        case "Jun34-Medium" : repFont = app.textFonts.getByName ("Jun34Pro-Medium"); break;
        case "Jun501-Bold" : repFont = app.textFonts.getByName ("Jun501Pro-Bold"); break;
        case "ShinseiKai-CBSK1" : repFont = app.textFonts.getByName ("ShinseiKaiPro-CBSK1"); break;
        default : repFont = app.textFonts.getByName (orgFont.name); //ないものはそのまま
      }//for switch
      //repFontで置換
      trgText[j].characterAttributes.textFont = repFont;
      //カーニングを自動に
      trgText[j].kerningMethod = AutoKernType.AUTO;
      //トラッキングをゼロに
      trgText[j].tracking = 0;
    }//for j
  }//for i

  //別名保存名を
  var removeExtName = docObj.name.toString().replace(/\..{2,4}$/,"");//拡張子をとる
  var oldName = removeExtName.replace(/ \[更新済み\]?/,"");// [更新済み]があるならとる
  var newPath = docObj.path.toString()+"/"+oldName+"_.eps";//アンダーバー付きに
  var savePath = new File(newPath);
  var saveOpt = new EPSSaveOptions();
  //ここから保存オプション
  saveOpt.compatibility = Compatibility.ILLUSTRATOR13;//AI CS3
  saveOpt.preview = EPSPreview.TRANSPARENTCOLORTIFF;//プレビューTIFFカラー（透明）
  //文字がちまちまいっぱいデータなんかはプレビューの限界に達すると思うのでそんなときはTIFF(不透明)に
  //saveOpt.preview = EPSPreview.COLORTIFF;//プレビューTIFFカラー（不透明）
  saveOpt.overPrint = PDFOverprint.PRESERVEPDFOVERPRINT;//オーバープリント保持
  saveOpt.embedAllFonts = true;//フォント埋め込む
  saveOpt.embedLinkedFiles = true;//配置画像を含む
  saveOpt.includeDocumentThumbnails = true;//サムネイル含める
  saveOpt.cmykPostScript = true;
  saveOpt.compatibleGradientPrinting = false;
  //saveOpt.flattenOutput = OutputFlattening.PRESERVEAPPEARANCE//
  saveOpt.EPSPostScriptLevelEnum = EPSPostScriptLevelEnum.LEVEL3//ポストスクリプトレベル3
  //別名保存、元データは保存せずに閉じる
  docObj.saveAs(savePath, saveOpt);
  docObj.close(SaveOptions.DONOTSAVECHANGES);
}//for document

alert("completed");
// var end = new Date().getTime();
// var lap = end-start;
// $.writeln(lap/1000+"sec");

```