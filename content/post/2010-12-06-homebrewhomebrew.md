+++
title = "Homebrewインストールメモ"
date = "2010-12-06T00:00:00+09:00"
tags = ["git", "homebrew"]

outdated = true
+++
パッケージ管理システムを Mac Ports から  [Homebrew](http://mxcl.github.com/homebrew/)  にした時のメモ。

[homebrew.png](/images/2010/11/homebrew.png)"

MacPortsのアンインストール

```bash
$ sudo port -f uninstall installed
```

opt/localとその他のフォルダの削除

```bash
$ sudo rm -rf \
    /opt/local \
    /Applications/DarwinPorts \
    /Applications/MacPorts \
    /Library/LaunchDaemons/org.macports.* \
    /Library/Receipts/DarwinPorts*.pkg \
    /Library/Receipts/MacPorts*.pkg \
    /Library/StartupItems/DarwinPortsStartup \
    /Library/Tcl/darwinports1.0 \
    /Library/Tcl/macports1.0 \
    ~/.macports
```

HomebrewのインストールをGitHubからRubyスクリプトで

```bash
ruby -e "$(curl -fsSL https://gist.github.com/raw/323731/install_homebrew.rb)"
```

Homebrewのアップデート自体Gitを使っているので、まずGitをインストール

```bash
$ brew search git
bagit    git-flow  git-sh    git-utils  willgit
git    git-hg    git-ssh    magit
git-extras  git-multipush  git-subtree  stgit
$ brew install git

$ brew update
From http://github.com/mxcl/homebrew
 * branch            master     -> FETCH_HEAD
Already up-to-date.

$ brew -v
0.7.1
```

Formula（パッケージ）が /usr/local/bin/ に入るのでパスを通しておく

<hr />

### 参考にした

- [2.5. Uninstall](http://guide.macports.org/chunked/installing.macports.uninstalling.html) 
- [MacPortsではなく、homebrewを使う - cipher](http://ash.roova.jp/cipher/2010/07/macportshomebrew.html) 
- [KOSHIGOE学習帳 - [system][osx] Homebrew 使い方メモ](http://w.koshigoe.jp/study/?%5Bsystem%5D%5Bosx%5D+Homebrew+%BB%C8%A4%A4%CA%FD%A5%E1%A5%E2#l1) 
- [Homebrew — MacPorts driving you to drink? Try Homebrew!](http://mxcl.github.com/homebrew/) 
- [Installation - homebrew - GitHub](https://github.com/mxcl/homebrew/wiki/installation) 