+++
title = "dotjsでlocalhost:3131しなくてよくする"
date = "2013-10-17T00:00:00+09:00"
tags = ["javascript"]

outdated = true
+++

[defunkt/dotjs](https://github.com/defunkt/dotjs) を有効にするのに Google Chrome を起動するたびに https://localhost:3131 を開いて「このまま続行」ボタンをクリックするのが面倒だったので、cliclick と AppleScript で起動時に開いてボタンをクリックするスクリプト [milligramme/prepare_dotjs_chrome](https://github.com/milligramme/prepare_dotjs_chrome) を使ってた

公式にやり方が追記されてたのでやってみた

[ssl instructions · d6acf62 · defunkt/dotjs](https://github.com/defunkt/dotjs/commit/d6acf625cec7be977c82d56d39c0ae2c443a87af)

> Now open https://localhost:3131 in Chrome and follow these steps:
> 
> - Click the "X" Padlock icon in the address bar
> - Click "Certificate Information"
> - Drag the large cert icon to your desktop
> - Open it with Keychain
> - Configure its Trust section as shown: http://cl.ly/Pdny

デスクトップに証明書アイコンをドロップ

![2013 10 17 Dotjs 01](/images/2013-10-17-dotjs-01.png)

キーチェーンに追加して、SSLを常に信頼にする

![2013 10 17 Dotjs 02](/images/2013-10-17-dotjs-02.png)

これで [OK](http://docs.komagata.org)

![2013 10 17 Dotjs 03](/images/2013-10-17-dotjs-03.png)