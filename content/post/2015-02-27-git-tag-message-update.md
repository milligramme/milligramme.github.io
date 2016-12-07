+++
title = "すでにつけた git tag のメッセージを更新したい"
date = "2015-02-27T00:00:00+09:00"
tags = ["git"]
+++

[git tag \- How do I edit an existing tag message in git? \- Stack Overflow](http://stackoverflow.com/questions/7813194/how-do-i-edit-an-existing-tag-message-in-git)

```
% git tag -n
v0.0.1          foo
v0.0.2          bar
v0.0.3          baz

% git tag v0.0.1 v0.0.1 -m poo
fatal: tag 'v0.0.1' already exists

% git tag v0.0.1 v0.0.1 -f -m poo
Updated tag 'v0.0.1' (was 2ad0e25)

% git tag -n
v0.0.1          poo
v0.0.2          bar
v0.0.3          baz
```