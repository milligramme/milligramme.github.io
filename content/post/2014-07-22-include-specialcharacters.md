+++
title = "InDesign.jsxでテキストにSpecialCharacterを含んでるかどうか?"
date = "2014-07-22T00:00:00+09:00"
tags = ["indesign", "extendscript"]
+++

テキスト中の SpecialCharacter の有無をみつける

forループは使いたくない

```js
text.characters.length !== text.characters.everyItem().contents.join("").length 
```

SpecialCharacterは 10文字の数字の enum になるので Character 数と一致しなかったら含む