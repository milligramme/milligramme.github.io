+++
title = "ざざっとFileをFolderでかき集めるjs"
date = "2009-10-14T00:00:00+09:00"
tags = ["extendscript"]
+++
フォルダ内にいっぱいある画像ファイルをファイル名の頭3文字を使った別のフォルダにまとめるJavaScript。

時間がなかったので、JavaScriptで強行しましたが、こういうファイル操作って、他のプログラミング言語だともっとスリムにできるのでしょうか。

もう一つぐらい老後のため覚えておきたいなぁ。

```js
//ターゲットのフォルダ
var imgFolder=Folder.selectDialog("choose target folder");
var imgFolder_fs=new File(imgFolder).fsName;
var imgFolder_lst = File(imgFolder_fs).getFiles("*");
if(imgFolder == false){
  alert("nothing");
exit();
}
//保存する親フォルダ
var saveFolder=Folder.selectDialog("choose folder to save");
var saveFolder_fs=new File(saveFolder).fsName;
if(saveFolder == false){
  alert("nothing");
exit();
}
//ファイル名から子フォルダをつくってそこにコピーしていく
for(var i=0; i < imgFolder_lst.length; i++){
  var imgFileName=imgFolder_lst[i].name;
  var file_postFix=myLeft(imgFileName, 3);
  var savePath=saveFolder_fs+"/"+file_postFix;
  var gatherFolder=(new Folder(savePath)).create();
  if(gatherFolder == true){
    imgFolder_lst[i].copy(savePath+"/"+imgFileName.toString());
  }
}
//Left関数みたいな
function myLeft(string, num){
  return string.substr(string.lenght-num, num);
}
```