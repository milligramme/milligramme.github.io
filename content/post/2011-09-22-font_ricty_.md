+++
title = "プログラミング用フォント Ricty を生成してみた"
date = "2011-09-22T00:00:00+09:00"
tags = ["development", "font"]

outdated = true
+++
プログラミング用フォント Ricty がよさそうだったのでを生成してみました。

![/images/2011/09/ricty_terminal.png](/images/2011/09/ricty_terminal.png)

### 準備

Ricty-3.1.1生成プログラムをダウンロード

[プログラミング用フォント Ricty](http://save.sys.t.u-tokyo.ac.jp/~yusa/fonts/ricty.html) 

Inconsolata.otf をダウンロード

[Inconsolata](http://www.levien.com/type/myfonts/inconsolata.html) 

Migu 1M をダウンロード

[ダウンロード : M+とIPAの合成フォント](http://mix-mplus-ipa.sourceforge.jp/download.html) 

fontforgeをインストール

```
$ brew install fontforge
==> Installing fontforge dependency: gettext
==> Downloading http://ftp.gnu.org/pub/gnu/gettext/gettext-0.18.1.1.tar.gz
######################################################################## 100.0%
==> Downloading patches
######################################################################## 100.0%
######################################################################## 100.0%
==> Patching
patching file gettext-tools/configure
patching file gettext-tools/Makefile.in
==> ./configure --disable-debug --prefix=/usr/local/Cellar/gettext/0.18.1.1 --wi
==> make
==> make install
==> Caveats
This formula is keg-only, so it was not symlinked into /usr/local.

OS X provides the BSD gettext library and some software gets confused if both are in the library path.

Generally there are no consequences of this for you.
If you build your own software and it requires this formula, you'll need
to add its lib & include paths to your build variables:

    LDFLAGS  -L/usr/local/Cellar/gettext/0.18.1.1/lib
    CPPFLAGS -I/usr/local/Cellar/gettext/0.18.1.1/include
==> Summary
/usr/local/Cellar/gettext/0.18.1.1: 368 files, 13M, built in 4.4 minutes
==> Installing fontforge dependency: glib
==> Downloading ftp://ftp.gnome.org/pub/gnome/sources/glib/2.28/glib-2.28.8.tar.
######################################################################## 100.0%
==> Downloading patches
######################################################################## 100.0%
######################################################################## 100.0%
######################################################################## 100.0%
######################################################################## 100.0%
######################################################################## 100.0%
######################################################################## 100.0%
==> Patching
patching file configure.ac
patching file glib-2.0.pc.in
patching file glib/gunicollate.c
patching file glib/gi18n.h
patching file gio/xdgmime/xdgmime.c
patching file gio/gdbusprivate.c
==> ./configure --disable-rebuilds --prefix=/usr/local/Cellar/glib/2.28.8 --with
######################################################################## 100.0%
==> ed - config.h < config.h.ed
==> make
==> make install
Warning: m4 macros were installed to "share/aclocal".
Homebrew does not append "/usr/local/share/aclocal"
to "/usr/share/aclocal/dirlist". If an autoconf script you use
requires these m4 macros, you'll need to add this path manually.
==> Summary
/usr/local/Cellar/glib/2.28.8: 348 files, 11M, built in 3.2 minutes
==> Installing fontforge dependency: pango
==> Downloading http://ftp.gnome.org/pub/GNOME/sources/pango/1.28/pango-1.28.4.t
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/pango/1.28.4 --with-x
==> make install
/usr/local/Cellar/pango/1.28.4: 127 files, 3.9M, built in 57 seconds
==> Installing fontforge dependency: potrace
==> Downloading http://potrace.sourceforge.net/download/potrace-1.9.tar.gz
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/potrace/1.9 --mandir=/usr/local/Cella
==> make install
/usr/local/Cellar/potrace/1.9: 8 files, 240K, built in 12 seconds
==> Installing fontforge
==> Downloading http://downloads.sourceforge.net/project/fontforge/fontforge-sou
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/fontforge/20110222 --enable-double --
==> make
==> make install
==> Caveats
fontforge is an X11 application.

To install the Mac OS X wrapper application run:
    brew linkapps
or:
    ln -s /usr/local/Cellar/fontforge/20110222/FontForge.app /Applications
==> Summary
/usr/local/Cellar/fontforge/20110222: 269 files, 15M, built in 2.5 minutes
```

ダウンロードした Ricty-3.1.1.tar.gz を展開して、

- Inconsolata.otf
- Migu-1M-bold.ttf
- Migu-1M-regular.ttf


を同じ階層にコピー

同梱されているシェルスクリプトを実行。引数は上記のフォントたち

ricty_generator.sh

```
$ sh ricty_generator.sh Inconsolata.otf Migu-1M-regular.ttf Migu-1M-bold.ttf
Ricty Generator 3.1.1

Author: Yasunori Yusa <lastname at save dot sys.t.u-tokyo.ac.jp>

This script is for generating ``Ricty'' font from Inconsolata and Migu 1M.
It requires 2-5 minutes to generate Ricty. Owing to SIL Open Font License
Version 1.1 section 5, it is PROHIBITED to distribute the generated font.

Generate modified Inconsolata
Open Inconsolata.otf
Generated Modified-Inconsolata-Regular.sfd
While bold-facing Inconsolata, wait a bit, maybe a bit...
Generated Modified-Inconsolata-Bold.sfd
Generate modified Migu 1M
Open Migu-1M-regular.ttf
While scaling Migu-1M-regular.ttf, wait a little...
Generated Modified-Migu-1M-regular.sfd
Open Migu-1M-bold.ttf
While scaling Migu-1M-bold.ttf, wait a little...
Generated Modified-Migu-1M-bold.sfd
Generate Ricty
While merging Modified-Inconsolata-Regular.sfd and Modified-Migu-1M-regular.sfd, wait a little more...
Generated Ricty-Regular.ttf
While merging Modified-Inconsolata-Bold.sfd and Modified-Migu-1M-bold.sfd, wait a little more...
Generated Ricty-Bold.ttf
Remove temporary files
Generated RictyDiscord-Regular.ttf
Generated RictyDiscord-Bold.ttf
Succeeded to generate Ricty!
\
```

成功すると4つのフォントが生成される

![/images/2011/09/ricty_generate.png](/images/2011/09/ricty_generate.png)

Ricty の他に Ricty Discord というのが生成されますが、説明によると

>  派生フォント Ricty Discord
>  Ricty では、調和・統一感の維持のため、プログラミング用フォントのコアである Inconsolata 由来の ASCII 文字に手を入れないようにしています。Discord (不協和音) 版は、統一感を乱す覚悟で ASCII 文字に手を入れた Ricty の派生フォントです。通常、Ricty Discord は Ricty 生成の際に自動的に生成されますが、パッチスクリプトを直接実行することによっても生成できます。

Ricty と Ricty Discord の比較

![/images/2011/09/ricty_ricty_discord.png](/images/2011/09/ricty_ricty_discord.png)

Discord の方が文字によって角がたった印象（rとか7とかDとか）、小さいサイズでみるとゴミゴミっぽく感じるのかも（好みだと思うが）。

とりあえず Terminal.app で使っている Menlo Regular のかわりに Ricty Reguler 使ってみた。同じサイズだと 2pt くらい小ぶりにみえるけど、あまり違和感もなく視認性はよさそう。

あと、小文字の **g** がいわゆる 「お団子g」 なのが個人的には好きだったりする。

TextMateの現在の仕様上、残念ながら使えない。

![/images/2011/09/ricty_on_textmate.png](/images/2011/09/ricty_on_textmate.png)