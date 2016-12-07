+++
title = "TextMate環境を新たに構築するためのメモ"
date = "2011-02-08T00:00:00+09:00"
tags = ["textmate"]

outdated = true
+++
普段コーディングに使っているTextMate、一年くらい使ってみて、BundlesやPluginsをいれてオレ様仕様に拡張拡張を繰り返してきて、同じ環境を作るときに大変そうなので再構築用のメモ。

<h3>日本語表示のためのプラグインとフォント</h3>
 [TextMate stuff - hetima.com](http://hetima.com/textmate/index.html) 
<ul>
  <li>CJK-Input.tmplugin : 日本語入力のための必須プラグイン</li>

  <li>ForMateKonaVe.ttf : 日本語表示のためのHalf-widthの日本語フォント</li>
</ul>
これらがないとまともに日本語が表示出来ない。

<h3>BundlesとPlugins</h3>
Bundlesはいっぱいあって面倒くさいので、Shellスクリプトにしてみる、あまり使用頻度が高くなくても、Bundles Editorで表示非表示できるのでとりあえず入れておく。

InDesignのスクリプト用の ExtendScript TextMate Bundleはforkしてスニペットを追加したもの。オリジナルは @kanemuさんの  [カネムーメモ: ExtendScript TextMate Bundle を公開しました。](http://blog.kanemu.net/2010/06/extendscript-textmate-bundle.html) 

<script src="https://gist.github.com/810974.js"> </script>

<h4>日本語を使う上での不具合を Bundle Editor で一部編集して微調整</h4>

![/images/2011/02/tm_special_return.png](/images/2011/02/tm_special_return.png)

**Enter確定ができなくなるスニペットの編集**
 [ウノウラボ by Zynga Japan: TextMateの設定メモ](http://labs.unoh.net/2010/03/textmatetips.html) 
HTML5 Bundle > Special: Return Inside Empty Open/Close Tags（Activation のフィールド内容を削除）

**全角スペースのハイライト**
 [TextMateで全角スペースと半角スペースの区別ができるようにする : アシアルブログ](http://blog.asial.co.jp/506) 

**Enter確定ができなくなるスニペットの編集**
 [カネムーメモ: Jsdoc-ToolkitをExtendScriptで試す](http://blog.kanemu.net/2010/05/jsdoc-toolkitextendscript.html) 
JsDoc Toolkit > Newline（Activation のフィールド内容をReturnからShift+Returnに変更）
今度は Homebrew でインストールしてみる
$ brew install jsdoc-toolkit

<h3>Finder ウィンドウのツールバー用アプリ・スクリプト</h3>
適当な場所（/Applications など）に保存しておいて、フォルダのツールバーにドラッグアンドドロップして配置
**Open in TextMate**
 [&quot;Open in TextMate&quot; from Leopard Finder – The Pug Automatic](http://henrik.nyh.se/2007/10/open-in-textmate-from-leopard-finder) 
ファイルまたはフォルダをTextMateで開くためのアプリ？
<ul>
  <li>1個だけ選択した場合は単一ウィンドウとして開く（プロジェクトドローワーなし）。</li>
  <li>2個以上のファイルをドラッグアンドドロップか、0個または2個以上のファイル選択してボタンクリックするとプロジェクトウィンドウとして開く。</li>
  <li>プロジェクトウィンドウで開いた場合は、Terminalでcdできるボタンが左下にできる。</li>
</ul>

**Open Terminal Here**

 [Marc Liyanage - Software - AppleScript - Open Terminal Here](http://www.entropy.ch/software/applescript/) 
フォルダをカレントディレクトリとしてTerminal.appで開く

![/images/2011/02/toolbar_app.png](/images/2011/02/toolbar_app.png)

<h3>Missing Drawer プラグイン</h3>
デフォルトでは、プロジェクトドローワーはウィンドウの表示位置によって、**右もしくは左から**にょきっと出てきます。この出方はあまり好きでない。
![/images/2011/02/tm_defaultdrawer.png](/images/2011/02/tm_defaultdrawer.png)
Missing Drawer Pluginをいれると、プロジェクトウィンドウを開いたときにファイルリストのドローワーがウィンドウ左側に一体化されます。
![/images/2011/02/tm_missingdrawer.png](/images/2011/02/tm_missingdrawer.png)
おまけで、左下に「** >_ **」とプロジェクトウィンドウをTerminalで開く（ $ cd [project folder] ）ボタンが追加される。

