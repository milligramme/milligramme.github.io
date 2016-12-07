+++
title = "リネームして再リンクなスクリプトたちを修正"
date = "2009-08-25T00:00:00+09:00"
tags = ["indesign"]
+++
つい先日に書いたInDesignの配置画像リネーム用のスクリプトを修正しました。

- [[InDesign]リネームして再リンク](/2009/08/19/indesign-relink-with-rename.html)  
- [[InDesign]リネームして再リンク（インターフェイス付き）](/2009/08/24/indesign-relink-with-rename-ui.html) 

リネームしたい名前が既にある時の処理で、そのファイルがドキュメントに配置され、

リンクパレットに表示されている場合でないとエラーになるのを修正しました。

![/images/2010/09/590-rep_error_memo.jpg](/images/2010/09/590-rep_error_memo.jpg)

例えば

"A.psd"を"C.psd"にリネームしようとする時、InDesignドキュメント上に"C.psd"がどこかしらに配置してあれば、

修正前のスクリプトでも動作しますが、"A.psd"と同階層のフォルダに"C.psd"があるのに、

ドキュメントに配置していない場合（パッケージ後に削除するなど）にエラーになってしまいます。


### 書き換えたところ

```js
//既に存在しているファイルのリンクを
//ドキュメントのリンクからひっぱってるのが問題
//無いのに有るみたいな矛盾が発生
//var existLink=app.documents[0].links.itemByName(nwFileName);
//var existLinkFile=File(existLink.filePath);
//現状のファイル名ファイルパスを置換するのに変更

var existLinkFilePath=currentLink.filePath.replace(currentFileName,nwFileName);
var existLinkFile=File(existLinkFilePath);
```

