+++
title = "TextMateのクリップボード履歴の無効化"
date = "2016-08-31T16:26:00+09:00"
tags = ["textmate"]

outdated = true 
+++

TextMateのクリップボード履歴、使ってないのにデフォルトで有効になっていて
dbファイル `~/Library/Application Support/TextMate/ClipboardHistory.db` が1GB強になっていた（2年半分）。

Alfred3の方でもクリップボード履歴の機能があり、そちらをつかっているので TextMate 側を無効にしようと

[Hidden Settings · textmate/textmate Wiki](https://github.com/textmate/textmate/wiki/Hidden-Settings)

    % defaults write com.macromates.TextMate.preview disablePersistentClipboardHistory -bool YES
    % defaults read com.macromates.TextMate.preview disablePersistentClipboardHistory 
    1
    

するものの、再起動などしてもまったく無効になる気配がない…

とりあえず、放置して様子見。

こまめに db消すしかないものだろうかと思ったが、翌日みたら
前日の履歴は残っていなかったので、こういう仕様なのでしょう。