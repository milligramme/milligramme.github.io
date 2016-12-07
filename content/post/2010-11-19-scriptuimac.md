+++
title = "垂直プログレスバー、スライダーはMac版でしか動作しない"
date = "2010-11-19T00:00:00+09:00"
tags = ["scriptui", "extendscript"]
+++
以前にInDesign用のスクリプトのインターフェイスとして何度か使用していた、

**ScriptUIでの垂直プログレスバーとスライダー**ですが、これは**Mac版でしか動作しません**、
**Windowsでは動作しない**ので注意が必要です。

またプログレスバーに関してはMac版でもIllustratorとPhotoshopでは経過表示に問題があってうまく動作しません。

### サンプル

```js
var win = new Window('dialog', "Verticals", undefined);
var p0,p1,p2,p3,p4, s0,s1,s2,s3,s4;

p0 = win.add('progressbar',[0,0,300,30], 0,100);
win.group = win.add('group')
p1 = win.group.add('progressbar',[0,0,30,120], 0,100);
p2 = win.group.add('progressbar',[0,0,30,120], 0,100);
p3 = win.group.add('progressbar',[0,0,30,120], 0,100);
p4 = win.group.add('progressbar',[0,0,30,120], 0,100);
s1 = win.group.add('slider',[0,0,30,120], 20, 0,100);
s2 = win.group.add('slider',[0,0,30,120], 40, 0,100);
s3 = win.group.add('slider',[0,0,30,120], 60, 0,100);
s4 = win.group.add('slider',[0,0,30,120], 80, 0,100);
s0 = win.add('slider',[0,0,300,30], 50, 0,100);
p0.value = 50;
p1.value = 20;
p2.value = 40;
p3.value = 60;
p4.value = 80;
win.show();
```

垂直なスライダーとプログレスバーはMac版でないと表示出来ません
![/images/2010/11/verticals.png](/images/2010/11/verticals.png)

きっかけは  [Peter Kahrel](http://www.kahrel.plus.com/)  さんからのメールでした。

2010-11-14 Peter Kahrelさんより「サイト内の垂直プログレスバーとスライダーが興味深いけど、日本語版の独自機能？」とメールをいただいた。

2010-11-15「progressbarとsliderのboundsをwidth < heightにするとできますよ」と教えてあげたところ

2010-11-15 Peter Kahrelさん「そうやってるけど、英語版のCS3〜5では動かないよ、あー残念だ。」との返答。この時点で、Mac/Winについて特に触れられてなく、てっきり「日本の文化、縦組みの特典？」などと思ったのでした。2バイトテキスト周りのバグバグバグとかいろいろあるし、それの副産物？とか…

[http://twitter.com/chalcedony/status/4381099685847041](http://twitter.com/chalcedony/status/4381099685847041)

2010-11-16 @chalcedony さんからInDesign CS4のWindows版でも動作しないって、教えてもらいました。（そういえば、ScriptUI for dummiesもWIndowスクリーンショットだった）そうなると英語版/日本語版というよりMac/Winの違いのようなので、あとは英語版Macでの動作がわかればすっきりするわけです。

2010-11-16「Windowsだと動かないみたいなのですが、英語版のMacではどうです？」のメールに対して

2010-11-17 Peter Kahrelさん「友人のMacで試したら問題なくできたよ、問題になるのはWindowsで、言語は関係ないんだねー」


#### 2010-11-22追記

@chalcedony さんからWinでのスクリーンショットをいただきました！あくまで水平を保とうとするわけですね。

[http://f.hatena.ne.jp/chalcedony_htn/20101122102634](http://f.hatena.ne.jp/chalcedony_htn/20101122102634) 

というわけで

### まとめ

垂直プログレスバーとスライダーがまともに動作するのはMac版のInDesignのみ

さて、これをバグとして報告するべきか否か。

個人的にResource Specificationsで表現出来ないので出来てるってことがバグって考えか？

ちなみに、Peter Kahrelさんは [こんな人](http://www.oreillynet.com/pub/au/2758) でした。 

[ScriptUI for dummies](http://www.kahrel.plus.com/indesign/scriptui.html)  

というリファレンスPDFは図版多めでシンプルなコード、平易な英語でわかりやすいのでScriptUIでダイアログとか作りたいと思ってる人にはオススメです。