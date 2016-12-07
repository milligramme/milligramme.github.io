+++
title = "TextMate2のQuickLookを無効にする"
date = "2013-11-20T00:00:00+09:00"
tags = ["quicklook", "textmate", "extendscript"]
+++

QuickLook で、テキストファイルがいつしか TextMate2 の Theme で表示されるようになってて、これがあまり **Quick**  じゃない（恐ろしくのろいときがある）のと、以前 [QLColorCode.qlgenerator](https://code.google.com/p/qlcolorcode/) を jsx にもシンタックスハイライトされるように改造したのに、上書きされてカラーリングされなくなってしまってたので、TextMate2 の QuickLook を無効にする方法を探してた

defaults での方法は無いみたく、単に app パッケージ内の

```
/Applications/TextMate.app/Contents/Library/QuickLook/TextMateQL.qlgenerator
```

をはずせばいいらしい。

あと、QLColorCode.qlgenerator の jsx 対応

```
/Library/QuickLook/QLColorCode.qlgenerator/Contents/Resources/\
  highlight/share/highlight/langDefs/js.lang
```

を複製して

```
/Library/QuickLook/QLColorCode.qlgenerator/Contents/Resources/\
  highlight/share/highlight/langDefs/jsx.lang
```
  
/Library/QuickLook/QLColorCode.qlgenerator/Contents/info.plist に追記

```xml
<dict>
  <key>UTTypeConformsTo</key>
  <array>
    <string>public.source-code</string>
  </array>
  <key>UTTypeDescription</key>
  <string>ExtendScript Source File</string>
  <key>UTTypeIdentifier</key>
  <string>com.adobe.jsx-source</string> # -- ここ適当
  <key>UTTypeTagSpecification</key>
  <dict>
    <key>public.filename-extension</key>
    <array>
      <string>jsx</string>
    </array>
  </dict>
</dict>
```

@nbqx さんのtweet で思い出したのでした

### 参考にした

- [TextMate 2 and Quick Look - Alex Chan](http://www.alexwlchan.net/2013/11/textmate-quick-look/)
- [QuickLookでソースをカラーリング：QLColorCodeの改良． - 趣日雑記](http://d.hatena.ne.jp/beehive62/20100802/1280739114)