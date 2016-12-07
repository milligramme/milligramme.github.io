+++
title = "斜体? 歪み?"
date = "2009-04-04T00:00:00+09:00"
tags = ["indesign", "extendscript"]

outdated = true
+++

[http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2714](http://www2.rocketbbs.com/11/bbs.cgi?id=thats&amp;mode=pickup&amp;no=2714) 

途中で斜体でなく歪みのころだと気付いた。

InDesign で「斜体」ってつかった事が無い、「別名保存」をしようとしてダイアログ自体は何度も出した経験有り。（cmd+shift+s [mac]）

```js
with (app.selection[0]){
/*斜体だとここで
  paragraphs[0].shataiMagnification  = 0;
  paragraphs[0].shataiDegreeAngle    = 4500;
  paragraphs[0].shataiAdjustRotation = false;
  paragraphs[0].shataiAdjustTsume    = true;
*/
//歪みだとこれ
  paragraphs[0].skew = 20;
}
```