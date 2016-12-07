+++
title = "new MacBook Air setup Pt.1"
date = "2013-10-02T00:00:00+09:00"
tags = ["osx", "dev"]
+++

増税前に新しい MacBook Air 11 を買ったのでセッティングメモ

古い MacBook Air 11 の方、環境がかなりひどい継ぎ接ぎ感が漂っていて、できるだけ新規にインストールするように心掛けた
なので、移行アシスタントでーとかはデータ類以外は基本的にやらない方針

### もろもろインストール
- Xcode
  - Command Line Tools
- Dropbox
- Alfred
- Google Chrome
- iTerm2
- TextMate2

### 旧環境からコピー
- keychains
  - ~/Library/Keychains/login.keychain -> キーチェーンアクセスから読み込み
- stickies
  - ~/Library/StickiesDatabase
- ssh
  - ~/.ssh/id_rsa -> コピーした後 `$ chmod 600` する
  - ~/.ssh/id_rsa.pub  
- fonts 
  - Ricty
  
### Homebrew インストール
- Homebrew
  - `ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"`

- brew install
  - git
  - tig

### ruby インストール
- brew install
  - openssl
  - readline
  - apple-gcc42
- github clone
  - rbenv
  - rbenv-build
    - `$ CONFIGURE_OPTS="--with-readline-dir=/usr/local --with-openssl-dir=/usr/local" rbenv install <ruby version>`
    - 1.9.3-p448
    - 2.0.0-p247

- gem install
  - bundler
  - travis
  - gisty
  
### node.js インストール
- nvm
  - `curl https://raw.github.com/creationix/nvm/master/install.sh | sh`
  - nvm install
    - 0.10.20 をインストール（偶数の安定版）
    - nvm use v0.10.20
- npm install
  - npm install -g coffee-script
  - npm install -g grunt-cli

### もろもろインストール
- brew install 続き
  - bash-complete
  - colordiff
  - tree
  - wget
  
- github clone
  - stringer -> rss reader
  - earthquake -> twitter client
    - consumer.yml
    - ~/.earthquake 以下プラグインなど

### 設定見直ししつつ ドットファイル
- .bashrc
- .bash_profile
- .gemrc
- .gitconfig
- .gitignore
- .irbrc
- .pryrc
- .tm_properties
- .vimrc

つづく

### 参考にした
- [Homebrew — MacPorts driving you to drink? Try Homebrew!](http://brew.sh/)
- [creationix/nvm](https://github.com/creationix/nvm#node-version-manager)
- [MacでSSH公開鍵・秘密鍵ファイルをコピーして使ったら警告がでた - アインシュタインの電話番号](http://blog.ruedap.com/2011/04/04/mac-ssh-key-copy-error)