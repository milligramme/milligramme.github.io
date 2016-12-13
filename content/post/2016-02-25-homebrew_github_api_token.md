+++
title = "HOMEBREW_GITHUB_API_TOKENを設定"
date = "2016-02-25T00:00:00+09:00"
tags = ["github", "homebrew","api"]
+++

homebrewで連続searchをしていたらGitHubのapi rate limitに引っかかった

```
% brew search alfred
Caskroom/cask/alfred
% brew search textmate
Caskroom/cask/textmate
% brew search iterm
No formula found for "iterm".
==> Searching pull requests...
Closed pull requests:
iTerm/iTerm2 support in mailtomutt (https://github.com/Homebrew/homebrew/pull/7817)
Error: GitHub API rate limit exceeded for 58.94.103.124. (But here's the good news: Authenticated requests get a higher rate limit. Check out the documentation for more details.)
Try again in 58 minutes 59 seconds, or create a personal access token:
  https://github.com/settings/tokens/new?scopes=&description=Homebrew
and then set the token as: HOMEBREW_GITHUB_API_TOKEN
```

1時間後にまたやるか Personal Access Tokenを作成ときかれるので

https://github.com/settings/tokens/new?scopes=&description=Homebrew

で

### Personal Access Tokenを作成

{{% img src="/images/2016-02-25-homebrew-github-api-token.png" %}}

取得したら環境変数に設定

```
export HOMEBREW_GITHUB_API_TOKEN=personal_access-tokens
```

