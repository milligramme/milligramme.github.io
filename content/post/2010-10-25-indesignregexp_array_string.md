+++
title = "親、祖父母フォルダを含んだパス書き出しをRegExp, Array, Stringで"
date = "2010-10-25T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++
InDesign内でリンク画像一覧をテキストで欲しいということでフルパスで書き出ししたら、「長い」とクレームがついた。
ということで、内包しているフォルダを含んだパスで書き出し、もしリンク画像が「Links」フォルダ内にあるときは、Linksフォルダを内包してるフォルダ付で書き出すようにする。正規表現、配列、文字列でどれがよいのか比較してみました。Linkオブジェクトの .filePath は OSX の場合、「:」で保持してるのを再認識、POSIXじゃなかったんだ。

**200個のリンクのドキュメントでためした結果**
![/images/2010/10/compare_regex_array_string.png](/images/2010/10/compare_regex_array_string.png)
正規表現が若干遅い。配列が一番シンプルに書けるような気がする。

```js
/**
 *  親フォルダ付リストを書き出す
 *  親フォルダ名が"Links"なら祖父母フォルダ付で書き出す
 *  
 *  Created by mg on 2010-10-25.
 */
var doc = app.documents[0];
var lin = doc.links;

//regex version
var s0 = new Date().getTime();
for (var i=0, iL=lin.length; i < iL ; i++) {
  arr = includeParentFolderPath_regex(lin[i].filePath, ":");
  $.writeln(arr);
};
var e0 = new Date().getTime();
$.writeln("===========================");

//array version
var s1 = new Date().getTime();
for (var i=0, iL=lin.length; i < iL ; i++) {
  arr = includeParentFolderPath_array(lin[i].filePath, ":");
  $.writeln(arr);
};
var e1 = new Date().getTime();
$.writeln("===========================");

//string version
var s2 = new Date().getTime();
for (var i=0, iL=lin.length; i < iL ; i++) {
  str = includeParentFolderPath_string(lin[i].filePath, ":");
  $.writeln(str);
};
var e2 = new Date().getTime();

//result
alert("compare\nRegExp:"+(e0-s0)+"\nArray:"+(e1-s1)+"\nString:"+(e2-s2));

//正規表現版fn
/**
 * include parent (grandparent) folder path RegExp ver
 * 
 * @param {String} path  ex. "/Users/gdansk/Desktop/temp/cf.indd"
 * @param {String} sp Separator ex. "/"
 */
function includeParentFolderPath_regex (path, sp) {
  var key3 = new RegExp("("+sp+"[^"+sp+"]+){3}$"); // (\/[^/]+){3}$
  var key2 = new RegExp("("+sp+"[^"+sp+"]+){2}$"); // (\/[^/]+){2}$
  var head = new RegExp("^"+sp);
  if(path.match(key2)[0].substr(1,5+sp.length).toLowerCase() === "links"+sp){
    return path.match(key3)[0].replace(head, "");
  }
  else{
    return path.match(key2)[0].replace(head, "");
  }
}

//配列版fn
/**
 * include parent (grandparent) folder path Array ver
 * 
 * @param {String} path  ex. "/Users/gdansk/Desktop/temp/cf.indd"
 * @param {String} sp Separator ex. "/"
 */
function includeParentFolderPath_array (path, sp) {
  var arr = path.split(sp);
  if(arr[arr.length-2].toLowerCase() === "links"){
    return arr[arr.length-3] + sp + arr[arr.length-2] + sp + arr[arr.length-1];    
  }
  else{
    return arr[arr.length-2] + sp + arr[arr.length-1];
  }
}

//文字列版fn
/**
 * include parent (grandparent) folder path String ver
 * 
 * @param {String} path  ex. "/Users/gdansk/Desktop/temp/cf.indd"
 * @param {String} sp Separator ex. "/"
 */
function includeParentFolderPath_string (path, sp) {
  if (path.substr(lastN(path, sp, 2) ,5+sp.length ).toLowerCase() === "links"+sp) {
    return path.substr(lastN(path, sp, 3), path.length - lastN(path, sp, 3));
  }
  else{
    return path.substr(lastN(path, sp, 2), path.length - lastN(path, sp, 2));
  }
}
/**
 * last N Index of
 * 
 * @param {String} str File Path
 * @param {String} sp Separator such as "/", ":"
 * @param {Number} count Position from end of string
 *  
 */
function lastN (str, sp, count) {
  var lp = str.length;
  for (var i=0; i < count; i++) {
    lp = str.lastIndexOf(sp, lp)-1;
  };
return lp + 2;
}
```