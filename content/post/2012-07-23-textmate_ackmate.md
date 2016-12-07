+++
title = "AckMateプラグインでプロジェクト内検索"
date = "2012-07-23T00:01:00+09:00"
tags = ["textmate"]

outdated = true
+++

### AckMateプラグイン

TextMateのプロジェクトをAckで検索できるプラグイン、ずいぶん前にいれていたのにぜんぜん使ってなくて、うっかりショートカットを押して発動したのでちゃんと調べてみた。

[![](/images/2012/07/ackmate_shortcut.png "ackmate_shortcut")](/images/2012/07/ackmate_shortcut.png)

### インストール

*   TextMateを終了させておく
*   [Downloads · protocool/AckMate ](https://github.com/protocool/AckMate/downloads "AckMate")のダウンロードページから zipをダウンロードして解凍
*   AckMate.tmplugin をダブルクリック（または `~/Library/Application\ Support/TextMate/Plugins` へコピー）
*   「Search Project With AckMate...」というショートカットができる

### .ackrc ファイルに設定を書き込む

素の状態だと、.jsx は無視されて検索対象にならないので、ホームディレクトリに .ackrc というファイルを作成して設定をしてあげる、ついでに.haml、ignorecase、検索結果の前後を表示など

```bash
--ignore-case
--after-context=1
--before-context=2

--type-set=javascript=.jsx,.jsxinc
--type-add=ruby=.haml
```

--type-set と --type-addの 違いは、'perl', 'ruby', 'php', 'python', 'shell', 'xml'はtypeが定義済みなのでsetできないのでaddするらしい

[![](/images/2012/07/ack_result.png "ack_result")](/images/2012/07/ack_result.png)

ショートカットは **command + option + control + f** (プロジェクト内でないと機能しない)