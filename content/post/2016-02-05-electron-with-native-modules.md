+++
title = "electronとNative Module"
date = "2016-02-05T00:00:00+09:00"
tags = ["electron", "nodejs"]
+++

[Module version mismatch\. Expected 47, got 46\. · Issue \#344 · polotek/libxmljs](https://github.com/polotek/libxmljs/issues/344)

> no helpful information here to debug this nor do I think you have done any googling around for how to resolve this issue. You upgraded node without rebuilding the module.

以前作った electron を流用して新しい electron アプリを作るときに、 Module versionの不一致というエラーがでて対処法をしらべた

nodejs は `v4.2.1`

[Previous Releases \| Node\.js](https://nodejs.org/en/download/releases/)

46がv4, 47がv5ということらしい

結果的に、Native Moduleを利用している時には electronの process.versionと実行するnodejsのバージョンがあっていないとダメらしい

electron -v => `v0.36.7`

process.version => `v5.1.1`



nvm で v5.1.1 をインストールし、nvm use 5.1.1 して npm install し直したら解決した

後で知った、もっと簡単な方法があったらしい

[electron/using\-native\-node\-modules\.md at master · atom/electron](https://github.com/atom/electron/blob/master/docs-translations/jp/tutorial/using-native-node-modules.md)

[electron/using\-native\-node\-modules\.md at master · atom/electron](https://github.com/atom/electron/blob/master/docs/tutorial/using-native-node-modules.md#using-native-node-modules)
