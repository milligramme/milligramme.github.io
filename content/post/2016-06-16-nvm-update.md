+++
title = "node.jsのアップデート"
date = "2016-06-16T10:06:00+09:00"
tags = ["nvm", "nodejs"]
+++

放置気味だった node.js環境を更新

## nvm自体をアップデート

```
% cd ~/.nvm
% git pull origin master
% nvm --version
0.29.0
```

## v4, v5, v6系をインストール

v4.2.1->v4.4.5に global modulesをインストールしながらインストール

[Autotomatic reinstall\-packages across node versions · Issue \#977 · creationix/nvm](https://github.com/creationix/nvm/issues/977)

```
% nvm install v4.4.5 --reinstall-packages-from=4.2.1

% nvm install v5.11.1 --reinstall-packages-from=5.1.1

% nvm install v6.2.1

% nvm ls
        v0.12.7
         v4.2.1
->       v4.4.5
         v5.1.1
         v6.2.1
default -> v4.4.5
node -> stable (-> v6.2.1) (default)
stable -> 6.2 (-> v6.2.1) (default)
iojs -> N/A (default)
```