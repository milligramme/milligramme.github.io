+++
title = "middleman-blogをmiddleman-deployでデプロイ"
date = "2016-01-18T00:00:00+09:00"
tags = ["middleman", "deploy"]
+++

middleman ver4にアップグレード後、githubのsrcブランチにpushすると
travis-ciで自動でビルド＆デプロイしてたのが動かなくなったので
[middleman\-contrib/middleman\-deploy](https://github.com/middleman-contrib/middleman-deploy)
に切り替えた

まだ

```rb
gem 'middleman-deploy', '~> 1.0'
```

だと middleman4 に対応していないので

```rb
gem 'middleman-deploy', '~> 2.0.0.pre.alpha'
```

を指定、config.rb の書き方も少しだけ違う

```
# 1.0
activate :deploy do |deploy|
  deploy.method = :git
end

# 2.0.0.pre
activate :deploy do |deploy|
  deploy.deploy_method = :git
end
```

