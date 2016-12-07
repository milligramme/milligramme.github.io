+++
title = "fswatchでxlsxをつくる"
date = "2016-06-30T11:00:00+09:00"
tags = ["fswatch", "rake", "xls", "csv"]
+++

Excel操作がつらいので、rubyXLでcsvをxlsxにするrubyスクリプトで rake task をつくった

    % fswatch /path/to/member.csv "rake create_xlsx"

fswatch でフォルダ監視して実行させる rake コマンドが動かなくて



[How to Use fswatch · emcrisostomo/fswatch Wiki](https://github.com/emcrisostomo/fswatch/wiki/How-to-Use-fswatch#piping-fswatch-output-to-another-process)

を参考にして

    % fswatch -0 /path/to/member.csv | xargs -0 -n 1 -I {} rake create_xlsx

としたら動いたのだけどなにかが間違っている気がする


