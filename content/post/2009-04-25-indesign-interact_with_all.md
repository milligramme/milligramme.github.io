+++
title = "interact with allの提案にのってみる"
date = "2009-04-25T00:00:00+09:00"
tags = ["extendscript", "indesign"]

outdated = true
+++
車に乗ったらシートベルトを締めるような感覚で

 [http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2879](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2879) 

せうぞーさんの提案の引用

>【提案】ダイアログを出すスクリプトを書く時は、最初にinteract with allを書きませんか？

 [ ブログにも書きました](http://d.hatena.ne.jp/seuzo/20080721/1216629501) が、これは本来、InDesignが再起動したら元に戻るべき設定です。

が、InDesignはその設定を捨ててくれない。

結果として、次以降のダイアログを出すスクリプトが失敗しつづけるわけです。

スクリプトだけじゃなくて、フォントやリンクのエラーダイアログも一切でなくなります。

そこで、ひとつ提案。
ダイアログを出すスクリプトを書くときは、一番最初にinteract with allを書いたらどうでしょう？
そうすれば、自分のスクリプトが失敗しないだけでなく、誤設定を修正できます。
AppleScriptならapplication直下で

```js
set user interaction level of script preferences to interact with all
```

JavaScriptなら、お〜まちさんが示したとおり、

```js
app.scriptPreferences.userInteractionLevel = UserInteractionLevels.interactWithAll;
```

とそれぞれ１行入れるだけです。

InDesignが再起動で設定を直してくれるまで、自由参加で。


ご自身のスクリプトもこの仕様で更新しておりました。この件、自分のスクリプト先生が以前に、万一の時のためにと同じように1行ファイルをくれてたので、interact with allの存在自体は知っていたけど、いまだに使う場面に巡りあってなかったのでした。