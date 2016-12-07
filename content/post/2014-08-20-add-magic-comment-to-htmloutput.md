+++
title = "TextMateのアップデート時にhtmloutput.rbにマジックコメントを追加する"
date = "2014-08-20T00:00:00+09:00"
tags = ["textmate"]

outdated = true
+++

TextMateがMarvericksのruby2が基準になっていてアップデートされるたびに [textmate/bundle-support.tmbundle](https://github.com/textmate/bundle-support.tmbundle) の htmloutput.rb のマジックコメントが消されて マークダウンプレビューが壊れて、不便なのでshellscriptを書いた

```bash
#!/usr/bin/env bash
rand=`uuidgen`
tmpfile=`mktemp $TMPDIR/$rand` || exit 1

set -e
trap 'echo NG; trap "rm -fv $tmpfile" EXIT' ERR

dir="$HOME/Library/Application Support/TextMate/Managed/Bundles/Bundle Support.tmbundle/Support/shared/lib/tm"

echo "# -*- coding: utf-8 -*-" > $tmpfile
cat $dir/htmloutput.rb >> $tmpfile
cat $tmpfile > $dir/htmloutput.rb

echo 'OK'
exit 0
```

### 参考にした
ファイルの行頭に足す

[Bashでの一時ファイルの取り扱い - rcmdnk's blog](http://rcmdnk.github.io/blog/2013/12/08/computer-bash/)

shellscriptでのtry~catch

[suz-lab - blog: シェルスクリプトで例外処理(try-catch文)的なもの](http://blog.suz-lab.com/2012/09/try-catch.html)
