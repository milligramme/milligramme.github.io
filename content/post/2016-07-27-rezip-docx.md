+++
title = "docx内のxmlを編集して再保存"
date = "2016-07-27T12:09:00+09:00"
tags = ["zip", "docx", "cli"]
+++

docx内のxmlを編集して、普通に再zipしても壊れたdocxができあがる。

    % cd /path/to/docx_unzipped_dir
    % mate word/document.xml
    % zip -r ../`date '+%F_%H%M%S.docx'` *
    
タイムスタンプつきで docxを生成していく。