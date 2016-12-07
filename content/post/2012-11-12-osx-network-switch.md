+++
title = "コマンドラインでネットワーク環境を切り替え"
date = "2012-11-12T00:00:00+09:00"
tags = ["osx", "cli"]

outdated = true
+++



システム環境設定 > ネットワーク でネットワーク環境：「自動」してると挙動があやしかったので、「自動」以外を作成して切り替えるようにしてみた。

ただ、

1.  システム環境設定 > ネットワーク または ネットワークメニューアイコンから"ネットワーク環境設定を開く.."
2.  ネットワーク環境：切り替え

ってGUIでの切り替えが面倒。

コマンドラインでの切り替え方を調べてみたら `scselect` コマンドでできるらしい

確認 (*が現在の設定)

```
$ scselect
Defined sets include: (* == current set)
 * 6716A142-4DW0-46BA-A702-1E95A98B1D07  (FUGU)
   C1960C4D-722B-4DF1-8FFF-A9AB8462A320  (Automatic)
   ABC39AD4-F38B-45AD-819E-847FAA36B824  (IKA)
```

設定

```
$ scselect IKA
CurrentSet updated to ABC39AD4-F38B-45AD-819E-847FAA36B824 (IKA)
```

適当にaliasを設定

```bash
alias ika="scselect IKA"
alias fugu="scselect FUGU"
```

Alfred から実行してもいい
[![](/images/2012/11/ika.png "ika")](/images/2012/11/ika.png)