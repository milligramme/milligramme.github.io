+++
title = "brew services で自動起動設定"
date = "2014-06-18T00:00:00+09:00"
tags = ["homebrew"]

outdated = true
+++

homebrew でインストールした mongodb, gitbucket などの自動起動を launchctl コマンドのかわりに `brew services` コマンドをつかって設定する

たとえば gitbucket

```
% brew info gitbucket
gitbucket: stable 2.0, HEAD
https://github.com/takezoe/gitbucket
/usr/local/Cellar/gitbucket/2.0 (3 files, 48M) *
  Built from source
From: https://github.com/Homebrew/homebrew/commits/master/Library/Formula/gitbucket.rb
==> Caveats
Note: When using launchctl the port will be 8080.

To reload gitbucket after an upgrade:
    launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.gitbucket.plist
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.gitbucket.plist
```

いままで launchctl で自動起動していた

brew services を使うと

```
% brew services
usage: [sudo] brew services [--help] <command> [<formula>]

Small wrapper around `launchctl` for supported formulae, commands available:
   cleanup Get rid of stale services and unused plists
   list    List all services managed by `brew services`
   restart Gracefully restart selected service
   start   Start selected service
   stop    Stop selected service

Options, sudo and paths:

  sudo   When run as root, operates on /Library/LaunchDaemons (run at boot!)
  Run at boot:  /Library/LaunchDaemons
  Run at login: /Users/gdansk/Library/LaunchAgents


% brew services start gitbucket
==> Successfully started `gitbucket` (label: homebrew.mxcl.gitbucket)

% brew services stop gitbucket
Stopping `gitbucket`... (might take a while)
==> Successfully stopped `gitbucket` (label: homebrew.mxcl.gitbucket)
```

すっきり