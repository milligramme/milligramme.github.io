+++
title = "書いたearthquake.gemプラグインをまとめる"
date = "2013-07-24T00:00:00+09:00"
tags = ["ruby", "twitter", "earthquakegem"]

outdated = true
+++

今までに練習でまじめに書いた earthquake.gem のプラグインをまとめてみる。Twitter API v1.1 になってから動作しないぽさそうなのは除く

### [RT数とFav数を表示するearthquake.gemプラグイン](https://gist.github.com/milligramme/5393644)

tweetの横に、Retweeted:(^n) Favorited:(*n) の數を表示するプラグイン、earthquake.gemの性格上ストリームで流れてくるものには意味がない、 :recent や :search コマンドとかで表示させたtweetsにでてくると思う（:status だとバグる）

```
⚡ :plugin_install https://gist.github.com/milligramme/5393644
```

![2013 07 24 Rt Fav Count](/images/2013-07-24-rt_fav_count.png)

### [タイムライン上のt.coを展開して表示するearthquake.gemプラグイン](https://gist.github.com/milligramme/5149099)

twitter の短縮url t.co がきもいので展開するプラグイン、でも t.coを展開したあとの htn.to の方がきもいのでそちらも何とかしたい。httpsからhttpにリダイレクトするタイプのurl（soundcloudとかvimeoとか）ではエラーになってt.coのままになります、not foundな t.coもエラーをはきます

```
⚡ :plugin_install https://gist.github.com/milligramme/5149099
```

![2013 07 24 Expand Tco](/images/2013-07-24-expand_tco.png)

##### 2013-09-10追記
`config[:expand_url] = true` にすれば、標準でできるようになってた（ただし公式RTなど一部のt.coはそのまま）


### [インストールされてるプラグインを表示するearthquake.gem プラグイン](https://gist.github.com/milligramme/3227201)
インストールしているプラグインを表示するプラグイン、ソースを表示し、:command があれば赤く表示（usage: `:show_plugins`）
    
```
⚡ :plugin_install https://gist.github.com/milligramme/3227201
```

![2013 07 24 Show Plugins](/images/2013-07-24-show_plugins.png)


### [プラグインを管理するearthquake.gem プラグイン](https://gist.github.com/milligramme/5253047)    
プラグインを有効無効にするプラグイン。.earthqauek/pluginsにxxxフォルダを生成して出し入れします。無効にするときにearthquakeがエラーを吐くのは仕様。（usage: `:manage_plugins [<on|off> <plugin_name.rb>]`）

```
⚡ :plugin_install https://gist.github.com/milligramme/5253047
```

![2013 07 24 Managa Plugin](/images/2013-07-24-managa_plugin.png)

