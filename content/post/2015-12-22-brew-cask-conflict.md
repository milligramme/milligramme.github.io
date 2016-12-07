+++
title = "brew-caskを新しいシステムに移行させる"
date = "2015-12-22T00:00:00+09:00"
tags = ["homebrew"]
+++

brew caskの仕組みが変わったようなので新しいシステムに移行させる

[caskroom/homebrew\-cask](https://github.com/caskroom/homebrew-cask#homebrew-cask)

[brew\-cask: move to using tap cmd directory\. by mikemcquaid · Pull Request \#15381 · caskroom/homebrew\-cask](https://github.com/caskroom/homebrew-cask/pull/15381)

[Changes to homebrew\-cask installation behaviour · Issue \#13201 · caskroom/homebrew\-cask](https://github.com/caskroom/homebrew-cask/issues/13201)

```
% brew doctor
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry and just ignore them. Thanks!

Warning: You have external commands with conflicting names.

Found command `brew-cask` in following places:
    /usr/local/bin/brew-cask
    /usr/local/Library/Taps/caskroom/homebrew-cask/cmd/brew-cask.rb

% brew uninstall --force brew-cask
Uninstalling brew-cask... (2,976 files, 1.5M)

% brew update
Already up-to-date.
```