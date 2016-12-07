+++
title = "git-push-f-head"
date = "2016-05-31T09:31:00+09:00"
tags = ["git"]
+++

トピックブランチをマージしてリリースしたのに、一度マージする前にもどさないといけなかった時の手順

    $ git push -f origin HEAD~:master

リモートをローカルのひとつ前の状態にもどす→ リモートに git push 強制

設定していた post-update hooks は反応しないので、手動で該当処理をする

### 参考にした

[Gitでやらかした時に使える19個の奥義 \- Qiita](http://qiita.com/muran001/items/dea2bbbaea1260098051)
