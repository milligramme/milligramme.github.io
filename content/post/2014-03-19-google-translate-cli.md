+++
title = "Google翻訳コマンドライン"
date = "2014-03-19T00:00:00+09:00"
tags = ["cli"]
+++

google翻訳のコマンドライン版をいれた

[soimort/google-translate-cli](https://github.com/soimort/google-translate-cli)

```
% git clone git://github.com/soimort/google-translate-cli.git
% cd google-translate-cli
% make install

% which translate
/usr/bin/translate
% which trs
/usr/bin/trs

% translate 忍者
env: gawk: No such file or directory
```

gawk が無いです

```
% brew install gawk

% which gawk
/usr/local/bin/gawk
```

### デフォルトでは英語へ翻訳

```
% translate 忍者
Ninja
% translate {zh=} 忍者
Ninja
% translate {=fr} 忍者
Ninja
% translate {ja=@ja} 忍者
Ninja
% translate {zh=ja} 忍者
忍者
% translate {=fi} 山 花 川 海 美しい 美味しい
vuori
kukka
joki
meri
kaunis
herkullinen
```

@を付与するとphonetically（音）として翻訳

### ファイルの指定

```
% trs {=fr} ~/Desktop/wagahai.txt
Je suis un chat. Pas encore de nom.
Mi当nécessaire et n'est pas de dire Où je suis né. Kototake qui est resté à pleurer Niyaniya à l'endroit où l' humide et sombre anything're dans le stockage.
```
