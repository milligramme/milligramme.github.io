+++
title = "グラデーション工作"
date = "2009-06-16T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++
![/images/2010/09/486-gradient_makegr.png](/images/2010/09/486-gradient_makegr.png)

![/images/2010/09/489-gradient_option.png](/images/2010/09/489-gradient_option.png)

誰しも何かしらのマイカラーチップを作って、持っていると思います。

それらを使ってグラデーションをつくるためのスクリプトです。

### 使い方

- カラーチップをページ内に適当ばらまきます
- 整列パネルで均等にそろえたり、お好きなように。
- チップを選択して実行すると、グラデーションのサンプルを作成し、グラデーション名を設定するダイアログが出ますので命名してあげてください
- グラデーションカラースウォッチが生成されます



こんな仕様です。

- ページ（スプレッド）の幅がグラデーションストップ100％になります
- ペーストボード上のカラーチップはページの幅の中に丸め込まれます
- たとえばA4見開き（W=420）の場合、カラーチップの左座標x=0がグラデーションストップ0%の位置、x=210が50%、x=420が100%になります。
- グラデーションのタイプ（円形か線形か）は、選択されたカラーチップの形状で決まります。一つ目のカラーチップが楕円オブジェクトなら円形グラデーション、それ以外の四角形、テキストフレーム、多角形、線のオブジェクトで作った場合は線形グラデーションになります。一つ目がどのオブジェクトになるかはよくわかりません（たぶんID順）ので、「シェイプを変換」ですべて同じ形状にしておけば問題ないでしょう。
- カラーチップで塗り[なし]、グラデーションはグラデーションストップに設定できないので、それぞれ[紙色]、[黒]として扱います。
- 濃淡スウォッチは、スウォッチとして登録した「○○○○○○　50％」などは大丈夫ですが、「○○○○○○」で濃度50％にしているだけのものは濃度100％の色として扱います。
- midpointはいまのところ50％固定です。x座標が一緒だとボケ足なしのパッキリグラデーション？になってしまいます。うまくやればトリコロールな国旗や日の丸の国旗ができます。

```js
/*
選択したかラーチップの塗りでグラデーション作成
"make gradient by color objects"
使い方：
グラーデーションを構成したい色のカラーチップをページ内に配置し、選択して実行します。
カラーチップの並べ方（スプレッド内の位置）でグラデーションストップの位置が決まります。
カラーチップが楕円なら、グラデーションタイプは「円形」、それ以外は「線形」になります。
グラデーションのサンプル作成後、グラデーション名を設定するダイアログがでます。
注意：
選択したカラーチップが同じX座標だと、ぱっきりグラデーションができます。
スウォッチとして保存されていない濃淡付きスウォッチは濃度100%扱いになります。
カラーチップがグラデーションの場合[黒]扱い、塗り[なし]は[紙色]扱いになります。
グループ化したカラーチップではエラーになります。
動作確認：OS10.4.11 InDesign CS2、CS3
milligramme(mg)
www.milligramme.cc
*/
//おまじない
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
(function(){
  var docObj=app.activeDocument;
  var pgWidth=docObj.documentPreferences.pageWidth;
  if(app.selection.length < 2){
    alert("色付きカラーチップを2個以上選択して下さい。");
  }
  else{
    //ルーラー原点をバックアップして「スプレッドに」
    var rulerBk=docObj.viewPreferences.rulerOrigin;
    var rulerTemp=1380143983;
    docObj.viewPreferences.rulerOrigin=rulerTemp;
    var selObj=docObj.selection;
    var count=selObj.length;
    //選択したカラーチップのあるページの親スプレッド内の子ページの数
    var parentPg=selObj[0].parent;
    var pageCntInSpred=parentPg.parent.pages.length;
    //グラデーションストップを作成。グラデーションを作成すると、
    //既に2つのストップが存在するのでその分を引いた数をつくる。
    var grObj=docObj.gradients.add();
    var grStp=grObj.gradientStops;
    for(var i=0; i < count-2; i++){
      grStp.add();
    }
    for(var j=0; j < count; j++){
      var selX=selObj[j].geometricBounds[1];
      //PB上のカラーチップはそれぞれ、ページの端に丸め込みます。
      if(selX < 0){         selX=0;       }       else if(selX > pgWidth*pageCntInSpred){
        selX=pgWidth*pageCntInSpred;
      }
      var selColr=selObj[j].fillColor;
      //塗り[なし]とグラデーションの時の処理
      //[なし]は[紙色]、グラデーションは[黒]に。
      switch(selColr.constructor.name){
        case "Swatch": selColr=docObj.swatches.item("Paper"); break;
        case "Gradient": selColr=docObj.swatches.item("Black"); break;
      }
      grStp[j].stopColor=selColr;
      grStp[j].location=100*selX/(pgWidth*pageCntInSpred);
    }
    //グラデーションサンプル作成と名前設定ダイアログ
    var rectObj=docObj.rectangles.add();
    rectObj.geometricBounds=[30,30,100,150];
    rectObj.fillColor=grObj;
    //命名空欄またはキャンセルなら「新規グラデーションスウォッチ〜〜」
    var setName=prompt("命名","");
    if(setName!=null){
      grObj.name=setName;
    }
    //カラーチップが楕円ならば、グラデーションタイプを「円形」にする
    switch(selObj[0].constructor.name){
      case "Rectangle":
      case "Polygon":
      case "TextFrame":
      case "GraphicLine":
        grTyp=1635282023; break;//linear（線形）;
      case "Oval":
        grTyp=1918985319; break;// radial（円形）;
    }
    //作ったグラデーションのタイプを設定
    var grID=grObj.id;
    docObj.gradients.itemByID(grID).type=grTyp;
    //ルーラーを戻す。
    docObj.viewPreferences.rulerOrigin=rulerBk;
  }
})();
```