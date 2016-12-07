+++
title = "gitのセキュリティアップデートと .gitconfingの変更"
date = "2014-12-26T00:00:00+09:00"
tags = ["git"]
+++

年末にむけて stringer のdbの7000行確認をしたら git が危ないバージョンといわれたのでアップグレードした

```
% heroku pg
Your version of git is 2.1.0. Which has serious security vulnerabilities.
More information here: https://blog.heroku.com/archives/2014/12/23/update_your_git_clients_on_windows_and_os_x

% brew outdated git
git (2.1.0 < 2.2.1)

% brew upgrade git
==> Upgrading 1 outdated package, with result:
git 2.2.1
==> Upgrading git
==> Downloading https://downloads.sf.net/project/machomebrew/Bottles/git-2.2.1.mountain_lion.bottle.tar.gz
######################################################################## 100.0%
==> Pouring git-2.2.1.mountain_lion.bottle.tar.gz
==> Caveats
The OS X keychain credential helper has been installed to:
  /usr/local/bin/git-credential-osxkeychain

The "contrib" directory has been installed to:
  /usr/local/share/git-core/contrib

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completion has been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/git/2.2.1: 1356 files, 32M
```

バージョンが変わるたびに .gitconfig の [pager] のpath変えていたけど

 `/usr/local/share/git-core/contrib` 使えばよかったみたい

```diff
# ~/.gitconfig
[pager]
-   log = /usr/local/Cellar/git/2.2.1/share/git-core/contrib/diff-highlight/diff-highlight | less
-   show = /usr/local/Cellar/git/2.2.1/share/git-core/contrib/diff-highlight/diff-highlight | less
-   diff = /usr/local/Cellar/git/2.2.1/share/git-core/contrib/diff-highlight/diff-highlight | less
+   log = /usr/local/share/git-core/contrib/diff-highlight/diff-highlight | less
+   show = /usr/local/share/git-core/contrib/diff-highlight/diff-highlight | less
+   diff = /usr/local/share/git-core/contrib/diff-highlight/diff-highlight | less
```

