+++
title = "routesで:collectionか:memberか"
date = "2013-06-20T00:00:00+09:00"
tags = ["rails"]
+++

config/routes.rb で `collection` か `member` か 時々忘れるのでメモ

```ruby
# config/routes.rb

resources :books do
  get 'download', :on => :member

  collection do
    get 'sold'
  end
end
```

memberは `:id` 付きのルートで、collectionは `:id` なしのルート。
