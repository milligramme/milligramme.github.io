+++
title = "段落・文字スタイルをテキスト書き出し"
date = "2009-04-15T00:00:00+09:00"
tags = ["indesign", "extendscript"]

outdated = true
+++

InDesignドキュメントの中で、段落スタイルが大量にあるときに、相互関係の確認用にテキスト書き出し。

■これからの課題

- 生成されたテキストファイルの2行目以降の行頭カンマが気になる。
- 項目が増えたらもっと別な考え方の方が良い気がする。

パラメーターはここを参考にしました。

お〜まちさんの [InDesign Object Model CS-CS3](http://www15.ocn.ne.jp/%7Epreopen/iddom/about.html) 

```js
/**
段落・文字スタイルのリストをテキスト書き出し
"export the list of characterStyle and paragraphStyle"
使い方：
実行するとデスクトップに最前面のドキュメントの段落スタイルと文字スタイルを
のリストのテキストを出力します。
テキスト出力のパスの関係上Mac用です。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme
www.milligramme.cc
*/
(function(){
  var docObj = app.activeDocument;
  var pStyle = docObj.paragraphStyles;
  var cStyle = docObj.characterStyles;
  //ヘッダにする内容、配列
  var pstList = ["スタイル名"+"\t"+"フォント名"+"\t"+"フォントスタイル"+"\t"
  +"サイズ"+"\t"+"水平倍率"+"\t"+"ぶらさがり"+"\t"+"行揃え"+"\t"+"左インデント"
  +"\t"+"禁則"+"\t"+"文字組アキ量"+"\r"];
  var cstList = ["スタイル名"+"\t"+"フォント名"+"\t"+"フォントスタイル"+"\t"
  +"サイズ"+"\t"+"色"+"\t"+"ベースラインシフト"+"\r"];
  //段落スタイル
  for (var i=0; i < pStyle.length ; i++){
    var pstName1 = pStyle[i].name;
    var pstName2 = pStyle[i].appliedFont.name;
    var pstName3 = pStyle[i].pointSize.toFixed(1)+"Q";
    var pstName4 = pStyle[i].horizontalScale.toFixed(1)+"%" ;
    //ぶらさがり
    var pstName5 = pStyle[i].kinsokuHangType;
    switch(pstName5){
      case 1248553062: pstName5 = "強制"; break;
      case 1248553074: pstName5 = "標準"; break;
      case 1852796517: pstName5 = "なし"; break;
      default:pstName5="その他";
    }
    //行揃え
    var pstName6=pStyle[i].justification;
    switch(pstName6){
      case 1633772147: pstName6 = "ノド元から整列"; break;
      case 1667591796: pstName6 = "中央揃え"; break;
      case 1667920756: pstName6 = "均等配置（最終行中央揃え）"; break;
      case 1718971500: pstName6 = "両端揃え"; break;
      case 1818584692: pstName6 = "左揃え"; break;
      case 1818915700: pstName6 = "均等配置（最終行左/上揃え）"; break;
      case 1919379572: pstName6 = "右揃え"; break;
      case 1919578996: pstName6 = "均等配置（最終行右/下揃え）"; break;
      case 1630691955: pstName6 = "ノド元に向かって整列"; break;
      default: pstName6="その他";
    }
    //左インデント
    var pstName7 = pStyle[i].leftIndent+"mm";
    //禁則
    var pstName8 = pStyle[i].kinsokuSet;
    switch(pstName8){
      case 1248357235: pstName8 = "強い禁則"; break;
      case 1851876449: pstName8 = "禁則を使用しない"; break;
      case 1249078131: pstName8 = "弱い禁則"; break;
      default: pstName85 = "その他";
    }
    //文字組アキ量
    var pstName9=pStyle[i].mojikumi;
    switch(pstName9){
      case 1246572848: pstName9 = "LineEndAllOneEmEnum" ;break;
      case 1246572593: pstName9 = "LineEndAllOneHalfEmEnum" ;break;
      case 1246572852: pstName9 = "LineEndPeriodOneEmEnum" ;break;
      case 1246572849: pstName9 = "LineEndUkeNoFloatEnum" ;break;
      case 1851876449: pstName9 = "Nothing" ;break;
      case 1246572598: pstName9 = "OneEmIndentLineEndAllNoFloatEnum" ;break;
      case 1246572597: pstName9 = "OneEmIndentLineEndAllOneEmEnum" ;break;
      case 1246572601: pstName9 = "OneEmIndentLineEndAllOneHalfEmEnum" ;break;
      case 1246572851: pstName9 = "OneEmIndentLineEndPeriodOneEmEnum" ;break;
      case 1246572599: pstName9 = "OneEmIndentLineEndUkeNoFloatEnum" ;break;
      case 1246572594: pstName9 = "OneEmIndentLineEndUkeOneHalfEmEnum" ;break;
      case 1246572596: pstName9 = "OneOrOneHalfEmIndentLineEndAllOneEmEnum" ;break;
      case 1246572850: pstName9 = "OneOrOneHalfEmIndentLineEndPeriodOneEmEnum" ;break;
      case 1246572600: pstName9 = "OneOrOneHalfEmIndentLineEndUkeNoFloatEnum" ;break;
      case 1246572595: pstName9 = "OneOrOneHalfEmIndentLineEndUkeOneHalfEmEnum" ;break;
      default: pstName9 = pStyle[i].mojikumi.name;
    }
    //プッシュ
    pstList.push( pstName1 + "\t" + pstName2 + "\t" + pstName3 + "\t" + pstName4 + "\t" + pstName5 + "\t" + pstName6 + "\t" + pstName7 + "\t" + pstName8 + "\t" + pstName9 + "\r" );
  } //for i
  //ここから文字スタイル
  for (var j=0; j < cStyle.length ; j++){
    var cstName1 = cStyle[j].name;
    var cstName2 = cStyle[j];
    if (cstName2 != "1851876449"){
      cstName2 = cstName2.appliedFont;
    }
    else{
      cstName2 = "変更なし";
    }
    var cstName3 = cStyle[j].fontStyle;
    if (cstName3 != "1851876449" || ""){
      cstName3 = cstName3;
    }
    else{
      cstName3 = "変更なし";
    }
    var cstName4 = cStyle[j].pointSize;
    if (cstName4 != "1851876449"){
      cstName4 = cstName4.toFixed(1)+"Q";
    }
    else{
      cstName4 = "変更なし";
    }
    var cstName5 = cStyle[j].fillColor ;
    if (cstName5 != null){
      cstName5 = cstName5.name;
    }
    var cstName6 = cStyle[j].baselineShift ;
    if (cstName6 != "1851876449"){
      cstName6=cstName6.toFixed(1)+"Q";
    }
    else{
      cstName6 = "変更なし";
    }
    //プッシュ
    cstList.push( cstName1 + "\t" + cstName2 + "\t" + cstName3 + "\t" + cstName4 + "\t" + cstName5 + "\t" + cstName6 +"\r" );
  }// for j
  //ここからファイル出力
  var fileObj = new File("~/desktop/style_list_text.txt")
  var flag = fileObj.open("w");
  if(flag == true){
    fileObj.writeln("段落スタイル" + "\r" + pstList + "\r" + "文字スタイル" + "\r" + cstList);
    fileObj.close();
  }
  else{
    alert("cant open file");
  }
})();
```