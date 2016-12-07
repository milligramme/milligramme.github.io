+++
title = "TextMate2 のバンドル管理"
date = "2012-10-24T00:00:00+09:00"
tags = ["textmate"]
+++

TextMate2 になってからバンドル、プラグイン、テーマの扱いが変わったのでメモ

### TextMate 1.x

```
$HOME/Library/Application\ Support/TextMate/Bundles
$HOME/Library/Application\ Support/TextMate/Plugins
$HOME/Library/Application\ Support/TextMate/Themes
```

のフォルダに.tmbundle, .tmplugin, .tmthemeファイルをそれぞれいれるかファイルをダブルクリックすることで自動でインストールされた

### TextMate 2.x

[TextMate Blog » Locating Bundles](http://blog.macromates.com/2011/locating-bundles/)
によると、自動インストール、自動アップデートしてくれるらしい

ver1に比べて管理するフォルダが増えたので、バンドルだけについてみると、使途不明のものをふくめて4つのBundlesフォルダがある

```
$HOME/Library/Application\ Support/TextMate/Managed/Bundles
$HOME/Library/Application\ Support/Avian/Pristine\ Copy/Bundles
$HOME/Library/Application\ Support/Avian/Bundles
$HOME/Library/Application\ Support/TextMate/Pristine\ Copy/Bundles
```

### バンドルについて仮定義

*   公式バンドル: Githubの[textmateリポジトリ](https://github.com/textmate/)または [avianのリポジトリ](https://github.com/avian/)で管理されたバンドル
*   サードパーティバンドル: 公開された上記バンドル以外
*   自前バンドル: 自分で作成したり、Forkして変更を加えたりするバンドル

とします。

### 公式バンドル

→TextMate/Managed/Bundles
Preferences...メニュー > Bundles のチェックボックスでインストール、アンインストールしてる
基本的にこのバンドルフォルダをいじる必要がない、自動でアップデートされる
依存するバンドルもいっしょにインストールされる

[![](/images/2012/10/pref_bundles.png "pref_bundles")](/images/2012/10/pref_bundles.png)

これらのバンドルはGithubのリポジトリから .tbz を

```
$HOME/Library/Caches/com.macromates.TextMate/Bundles
```

にダウンロードしてキャッシュ、 TextMate/Managed/Bundles に展開しているみたい。
なので、Githubが落ちてたり、オフラインだとキャッシュに無い新しい言語のバンドルはインストールできない。

### サードパーティバンドル

→Avian/Pristine\ Copy/Bundles
バンドルファイルをダブルクリックまたはアプリにドラッグアンドドロップした場合、ここに入る。**そのままつかうならここに入れればいい**。

[![](/images/2012/10/install_extendscript.png "install_extendscript")](/images/2012/10/install_extendscript.png)

### 自前バンドル

→Avian/Bundles
**自分でつくったバンドル**またはForkしたバンドルですべての変更をコミットしたい場合はここに手動でコピーする。
また、
→Avian/Bundles
には公式バンドル・サードパーティバンドルに変更を加えた場合の差分が保存される。
Bundles > Edit Bundles... のチェックボックスで、オンオフした場合もここに差分が生成される（info.plistにisDisableフラグが記述されたのバンドルファイルが追加される）

[![](/images/2012/10/haml_lang.png "haml_lang")](/images/2012/10/haml_lang.png)

TextMate/Managed/Bundles/ および Avian/Pristine\ Copy/Bundlesフォルダ内のバンドルがアップデートされても差分の変更内容は Avian/Bundles に保存されてるので消えない仕組みになっているようです。逆に変更を消したかったら差分のバンドルを捨てたらいい。

### まとめ

*   自前バンドルは **Avian/Bundles** にいれる
*   普通にそのまま使うなら **Avian/Prestine\ Copy/Bundles** にいれる（にはいる）
*   TextMate/Managed/Bundles はいじらない
*   TextMate/Pristine\ Copy/Bundles は存在理由がよくわからない

[![](/images/2012/10/tmbundle.png "tmbundle")](/images/2012/10/tmbundle.png)

> pristine /prístiːn/
> 形容詞 ｟限定｠
> 1 ｟文語｠（純粋さがまだ保たれていた）初期の, 原始の(primitive).
> 2 純朴な, 無垢（むく）の；新品の；（食べ物などが）手つかず状態の.

テーマとプラグインについては、ver.1のテーマは変換が必要、プラグインは対応したプラグインあるんだろうか、まだみたことないです。