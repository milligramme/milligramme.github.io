+++
title = "Readonly なMac版Officeをhomebrewでインストール"
date = "2015-02-08T00:00:00+09:00"
tags = ["homebrew", "osx"]
+++

[Mac版Officeは読み取り専用なら無料 \- komagata](http://docs.komagata.org/5237)

```
% brew cask install microsoft-office
```

これだと en-US 版が入る。ja-JPがいい場合

[homebrew\-cask/microsoft\-office\.rb at master · caskroom/homebrew\-cask](https://github.com/caskroom/homebrew-cask/blob/master/Casks/microsoft-office.rb#L5)

5行目のurlを変更して ja-JP 版を手に入れられる

http://officecdn.microsoft.com/pr/MacOffice2011/ja-JP/MicrosoftOffice2011.dmg