+++
title = "TextMateとRictyでCTFontCreateWithName"
date = "2016-04-13T00:00:00+09:00"
tags = ["textmate", "font", "ricty"]
+++

[yascentur/Ricty: Ricty \-\-\- fonts for programming](https://github.com/yascentur/Ricty)
 を v3.2.2からv4.0.1に差替えた後で、Console.appを使ってて気づいたのだけど

    2016/04/13 9:30:32.209 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:32.212 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:32.448 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:32.456 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.090 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.280 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.314 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.314 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.315 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/04/13 9:30:33.316 TextMate[1004]: CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.

    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.
    2016/03/31 19:20:24.724	TextMate[4411]	CoreText performance note: Client called CTFontCreateWithName() using name "Ricty Regular" and got font with PostScript name "Ricty-Regular". For best performance, only use PostScript names when calling this API.

みたいのが大量に出るようになっていた。

TextMate は v2.0-beta.9

```
# .tm_properties
# Default
fontName         = "Ricty Regular"
```

- v3.2.2にもどしても出るので以前からあった問題？
- Menlo など他のフォントにしてみるとでないので、 Ricty + TextMateの相性問題？

と思ったら

- [Fixing Qt 4 for Mac OS X 10\.9 \| Successful Software](https://successfulsoftware.net/2013/10/23/fixing-qt-4-for-mac-os-x-10-9-mavericks/)
- [CoreText Issue on OS X Mavericks\. For best performance, only use PostScript names when calling CTFontCreateWithName\(\) · Issue \#11418 · ariya/phantomjs](https://github.com/ariya/phantomjs/issues/11418)

```
# .tm_properties
# Default
fontName         = "Lucida Grande"
```

    2016/04/13 11:35:03.722 TextMate[8219]: CoreText performance note: Client called CTFontCreateWithName() using name "Lucida Grande" and got font with PostScript name "LucidaGrande". For best performance, only use PostScript names when calling this API.

- PostScript名で呼び出せっていってるので、その名前に差違があるフォントだとでるのかもしれない
- Yosemiteでは起きないので Marvericksの問題？


```
# .tm_properties
# Default
fontName         = "Ricty-Regular"
```

PostScript名で fontName を設定しても改善せず

> For best performance ...

となっているので、しばらく気にしないでおくことにした。