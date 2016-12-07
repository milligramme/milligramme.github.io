+++
title = "TextMateでバックグラウンドを透過させる"
date = "2016-08-10T10:42:00+09:00"
tags = ["textmate"]

outdated = false 
+++

TextMateでエディタ画面を透過させたいが opacity: .8 みたいのは効かない。

バンドルの編集で

Bundles > Edit Bundles > RailsCast > Themes > RailsCast

Railscasts Theme（[ryanb/textmate\-themes: My TextMate themes \(includes Railscasts theme\)](https://github.com/ryanb/textmate-themes)）を v1からv2用に変換したものを使っている。


`settings.settings.background` でアルファチャンネルを設定すればいい

dd => 0.866666666666666..

```
{	gutterSettings = {
		foreground = '#858179';
		background = '#45433Edd'; <== アルファチャンネル
		divider = '#75715E';
		selectionBackground = '#3B3935';
		selectionForeground = '#BFBBB0';
	};
	settings = (
		{	settings = {
				foreground = '#E6E1DC';
				background = '#2B2B2Bdd'; <== アルファチャンネル
				caret = '#FFFFFF';
				invisibles = '#404040';
				selection = '#5A647EE0';
				lineHighlight = '#333435';
			};
		},
```

### 参考にした

- [osx \- Opacity of background in Textmate2 \- Stack Overflow](http://stackoverflow.com/questions/20537001/opacity-of-background-in-textmate2)
