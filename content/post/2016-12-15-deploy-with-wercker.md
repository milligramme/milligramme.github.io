+++
outdated = false
title = "Hugo-blogをGitLab Pagesにデプロイする"
description = ""
tags = [
"hugo","wercker", "deploy"
]
date = "2016-12-15T20:07:05+09:00"
categories = [
]
draft = true

+++

Hugoを github pagesにデプロイする前に

[Hugo \- Automated deployments with Wercker](https://gohugo.io/tutorials/automated-deployments/)

themeを調整したりするため、gitlab pages をためす。

[Hosting on GitLab\.com with GitLab Pages \| GitLab](https://about.gitlab.com/2016/04/07/gitlab-pages-setup/)

.gitlab-ci.yml

```yaml
image: publysher/hugo

pages:
    script:
        - hugo
    artifacts:
        paths:
          - public
    only:
        - master
```



Werckerいらずで、リポジトリにプッシュすると、そのまま build＆deploy してくれる

https://username.gitlab.io のようなurlになる

