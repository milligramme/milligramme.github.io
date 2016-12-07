+++
title = "changepagestring.pl でpdf内の文字列置換"
date = "2016-09-01T23:30:00+09:00"
tags = ["perl", "pdf"]

outdated = false 
+++

pdf内の文字列を変更したくて、AcrobatのJavaScriptでできないか調べてみたが、
どうもつらそうだったので、別アプローチ

[changepagestring\.pl \- search\.cpan\.org](http://search.cpan.org/dist/CAM-PDF/bin/changepagestring.pl)

でできそうということで

[CAM::PDF \- search\.cpan\.org](http://search.cpan.org/~cdolan/CAM-PDF-1.58/lib/CAM/PDF.pm)

    % cpan CAM::PDF

`$HOME/perl5/bin` に changepagestring.pl が含まれる。

一度リロードしてパスを通す。

```
changepagestring.pl /path/to/target.pdf find-string change-string ~/Desktop/`uuidgen`.pdf
```

これで 61B380D9-8656-42A3-B9E8-5923FFA54D51.pdf のようなpdfが作成させる。

InDesignから生成したlayerのpdfだとだめなので、一度レイヤー統合などの最適化させる必要あり。
 
日本語混じりだと絶望的にレイアウトがくずれるので、やっぱりだめ。


