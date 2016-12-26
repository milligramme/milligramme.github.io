+++
outdated = false
date = "2016-12-21T21:57:27+09:00"
title = "aerobatic bitbucket addon"
description = ""
tags = ["hugo","bitbucket"
]
categories = [
]
draft = false

+++



bitbucket-pages + hugo + wercker を試そうと [Hugo Continuous Integration With Wercker And Bitbucket](https://www.aerobatic.com/blog/hugo-continuous-integration-with-wercker-aerobatic-and-bitbucket) をみてたら、デフォルトで hugoサポートされたので不要っぽい記述があった。

[Easy Continuous Deployment of Hugo Sites](https://www.aerobatic.com/blog/easy-hugo-continuous-deployment)

[Aerobatic Hosting for Bitbucket \| Atlassian Marketplace](https://marketplace.atlassian.com/plugins/aerobatic-bitbucket-addon/cloud/overview)


aerobatic-bitbucket-addon というアドオンを利用する。

![2016 12 21 Aerobatic Menu](/images/2016-12-21-aerobatic-menu.png)

以下の package.json を用意すると

```json
{
  "_aerobatic": {
    "build": {
      "engine": "hugo",
      "themeRepo": "https://github.com/digitalcraftsman/hugo-cactus-theme"
    }
  }
}
```

username.bitbucket.org でなく username.aerobatic.io にデプロイしてくれる。


hugoのバージョンが0.16なので、min_ver = 0.17 のテーマが使えなかった

![2016 12 21 Aerobatic Log](/images/2016-12-21-aerobatic-log.png)

フリープランは１日５デプロイで2サイト1ドメインまで https://www.aerobatic.com/pricing/

