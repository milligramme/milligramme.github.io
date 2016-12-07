+++
title = "Stringerでlazy_fetchが効かない"
date = "2014-09-25T00:00:00+09:00"
tags = ["heroku"]
+++

[swanson/stringer](https://github.com/swanson/stringer) の scheduler でまわしてる `rake lazy_fetch` が機能してないっぽいのをしらべた

```
% heroku run rake lazy_fetch --trace
Running `rake lazy_fetch --trace` attached to terminal... up, run.7914
** Building /js/application.js...
** Building /css/application.css...
** Invoke lazy_fetch (first_time)
** Execute lazy_fetch
rake aborted!
bad URI(is not URI?): 9:http://xxxxxxxx.herokuapp.com/
/app/vendor/ruby-2.0.0/lib/ruby/2.0.0/uri/common.rb:176:in `split'
/app/vendor/ruby-2.0.0/lib/ruby/2.0.0/uri/common.rb:211:in `parse'
/app/vendor/ruby-2.0.0/lib/ruby/2.0.0/uri/common.rb:747:in `parse'
/app/vendor/ruby-2.0.0/lib/ruby/2.0.0/uri/common.rb:996:in `URI'
/app/Rakefile:26:in `block in <top (required)>'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:236:in `call'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:236:in `block in execute'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:231:in `each'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:231:in `execute'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:175:in `block in invoke_with_call_chain'
/app/vendor/ruby-2.0.0/lib/ruby/2.0.0/monitor.rb:211:in `mon_synchronize'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:168:in `invoke_with_call_chain'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/task.rb:161:in `invoke'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:149:in `invoke_task'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:106:in `block (2 levels) in top_level'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:106:in `each'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:106:in `block in top_level'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:115:in `run_with_threads'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:100:in `top_level'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:78:in `block in run'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:165:in `standard_exception_handling'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/lib/rake/application.rb:75:in `run'
/app/vendor/bundle/ruby/2.0.0/gems/rake-10.1.1/bin/rake:33:in `<top (required)>'
/app/vendor/bundle/ruby/2.0.0/bin/rake:23:in `load'
/app/vendor/bundle/ruby/2.0.0/bin/rake:23:in `<main>'
Tasks: TOP => lazy_fetch
```

ENV['APP_URL'] の頭にへんな数字が入っていてエラーになっていた

stringerの最初の設定で

```
heroku config:set APP_URL=`heroku apps:info | grep -o 'http[^"]*'`
```

でアプリのurl取得するときに `grep` が

```
alias grep='grep -n --color=auto'
```

になってたのが原因