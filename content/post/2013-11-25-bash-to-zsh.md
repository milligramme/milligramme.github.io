+++
title = "bashからzshに乗り換えメモ"
date = "2013-11-25T00:00:00+09:00"
tags = ["zsh", "bash"]
+++

### bashからzshに乗り換えたメモ

```
.bashrc
.bash_profile
.dots/ 
```

.bash_profile から .bashrc を読み込むようにしている。 .dots/ に 設定類(function, config, alias) を分けていれてて .bashrc で読み込むようにしてる。とりあえず、これらをzsh用に複製

```
$ cp .bashrc .zshrc
$ cp .bash_profile .zsh_profile
$ cp -r .dots .zdots
```

.zsh_profile に .zshrc を読みに行くように記述

```
$ chsh `which zsh`
$ echo $SHELL
```

プロンプト（見た目）が変わるのといままでの補完（bash-completion）が効かなくなるけど zsh に切り替わった