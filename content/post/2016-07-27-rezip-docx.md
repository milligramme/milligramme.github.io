+++
title = "docx内のxmlを編集して再保存"
date = "2016-07-27T12:09:00+09:00"
tags = ["zip", "docx", "cli"]
+++


osxでunzipしたdocx内のxmlを編集、zipして再びdocxにもどしたい。

そのまま `/System/Library/CoreServices/Applications/Archive Utility.app` とかでzipすると壊れたdocxができあがる。

## コマンドラインの zip を使う

    % cd /path/to/docx_unzipped_dir
    % mate word/document.xml
    % zip -r ../`date '+%F_%H%M%S.docx'` *
    
タイムスタンプつきで docxを生成していく。