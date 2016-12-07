+++
title = "middleman-syntaxでrougeがwarningだす"
date = "2015-07-23T00:00:00+09:00"
tags = ["ruby", "middleman"]
+++

middleman-blog を bundle update したら rouge がwarningを出すようになった

```
% be middleman
== The Middleman is loading
/Users/gdansk/.rbenv/versions/2.0.0-p576/lib/ruby/gems/2.0.0/gems/rouge-1.9.1/lib/rouge/lexers/shell.rb:20: warning: already initialized constant Rouge::Lexers::Shell::KEYWORDS
/Users/gdansk/.rbenv/versions/2.0.0-p576/lib/ruby/gems/2.0.0/gems/rouge-1.9.1/lib/rouge/lexers/shell.rb:20: warning: previous definition of KEYWORDS was here
/Users/gdansk/.rbenv/versions/2.0.0-p576/lib/ruby/gems/2.0.0/gems/rouge-1.9.1/lib/rouge/lexers/shell.rb:25: warning: already initialized constant Rouge::Lexers::Shell::BUILTINS
/Users/gdansk/.rbenv/versions/2.0.0-p576/lib/ruby/gems/2.0.0/gems/rouge-1.9.1/lib/rouge/lexers/shell.rb:25: warning: previous definition of BUILTINS was here
```

middleman-syntax でrequireしてる

- [Use require instead of load to handle double loading by segiddins · Pull Request \#291 · jneen/rouge](https://github.com/jneen/rouge/pull/291)
- [uninitialized constant \#<Class:\.\.\.>::Rouge · Issue \#45 · middleman/middleman\-syntax](https://github.com/middleman/middleman-syntax/issues/45)

っぽい

