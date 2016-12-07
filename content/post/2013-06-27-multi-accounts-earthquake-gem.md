+++
title = "earthquake.gem をマルチアカウントで使う"
date = "2013-06-27T00:00:00+09:00"
tags = ["twitter", "ruby", "earthquakegem"]

outdated = true
+++

デフォルトでは設定フォルダ ~/.earthquake を使用する、起動時の引数で変更可能

```bash
$ cat ~/.earthquake/config
Earthquake.config[:token] = ACCESS_TOKEN
Earthquake.config[:secret] = ACCESS_TOKEN_SECRET

$ cp -r ~/.earthquake ~/.eq_another_account
```

earthquake.gem を設定フォルダを指定して起動

```bash
$ earthquake ~/.eq_another_account
```

twitterで再認証

```bash
⚡ :reauthorize
1) open: https://api.twitter.com/oauth/authorize?oauth_token=OAUTH_TOKEN
2) Enter the PIN: PIN_PIN_PIN
Saving 'token' and 'secret' to '/Users/gdansk/.eq_another_account/config'
```
 
上書きでなく追記されるので古い方は削除

```bash
$ cat ~/.eq_another_account/config
Earthquake.config[:token] = OLD_ACCESS_TOKEN
Earthquake.config[:secret] = OLD_ACCESS_TOKEN_SECRET
Earthquake.config[:token] = NEW_ACCESS_TOKEN
Earthquake.config[:secret] = NEW_ACCESS_TOKEN_SECRET
```

historyを初期化して、プラグインをデフォルトフォルダからシンボリックリンクをはって共用

    $ echo '' > ~/.eq_another_account/history
    $ rm -fr ~/.eq_another_account/plugin/
    $ ln -s ~/.earthquake/plugin/ ~/.eq_another_account/plugin

    $ earthquake ~/.eq_another_account

アカウントごとにわかりやすいエイリアスつくってればよさそう
    
[jugyo/earthquake.gem](https://github.com/jugyo/earthquake) 便利