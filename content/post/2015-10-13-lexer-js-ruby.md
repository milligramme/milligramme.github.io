+++
title = "jsとrubyで構文解析"
date = "2015-10-13T00:00:00+09:00"
tags = ["javascript", "ruby"]
+++

構文解析

[polygonplanet/Chiffon](https://github.com/polygonplanet/Chiffon)

A very small ECMAScript parser, tokenizer and minify written in JavaScript.

```js
var Chiffon = require('chiffon');

var code = "#target 'InDesign'\nvar doc = app.documents[0];\nvar foo=1;\t\tif (foo === 0){ doc.textFrames.add() }";
var tokens = Chiffon.tokenize(code);
console.log(tokens);

var unto = Chiffon.untokenize ( tokens );
console.log(unto);

var minim = Chiffon.minify(code);
console.log(minim);

// % node index.js
// [ { type: 'Identifier', value: '#target' },
//   { type: 'String', value: '\'InDesign\'' },
//   { type: 'Keyword', value: 'var' },
//   { type: 'Identifier', value: 'doc' },
//   { type: 'Punctuator', value: '=' },
//   { type: 'Identifier', value: 'app' },
//   { type: 'Punctuator', value: '.' },
//   { type: 'Identifier', value: 'documents' },
//   { type: 'Punctuator', value: '[' },
//   { type: 'Numeric', value: '0' },
//   { type: 'Punctuator', value: ']' },
//   { type: 'Punctuator', value: ';' },
//   { type: 'Keyword', value: 'var' },
//   { type: 'Identifier', value: 'foo' },
//   { type: 'Punctuator', value: '=' },
//   { type: 'Numeric', value: '1' },
//   { type: 'Punctuator', value: ';' },
//   { type: 'Keyword', value: 'if' },
//   { type: 'Punctuator', value: '(' },
//   { type: 'Identifier', value: 'foo' },
//   { type: 'Punctuator', value: '===' },
//   { type: 'Numeric', value: '0' },
//   { type: 'Punctuator', value: ')' },
//   { type: 'Punctuator', value: '{' },
//   { type: 'Identifier', value: 'doc' },
//   { type: 'Punctuator', value: '.' },
//   { type: 'Identifier', value: 'textFrames' },
//   { type: 'Punctuator', value: '.' },
//   { type: 'Identifier', value: 'add' },
//   { type: 'Punctuator', value: '(' },
//   { type: 'Punctuator', value: ')' },
//   { type: 'Punctuator', value: '}' } ]
// #target'InDesign'var doc=app.documents[0];var foo=1;if(foo===0){doc.textFrames.add()}
// #target'InDesign'
// var doc=app.documents[0];var foo=1;if(foo===0){doc.textFrames.add()}
```

---

rubyは標準ライブラリである

[class Ripper::Lexer \(Ruby 2\.2\.0\)](http://docs.ruby-lang.org/ja/2.2.0/class/Ripper=3a=3aLexer.html)

```rb
require 'ripper'
require 'pp'

code = File.read(__FILE__)
rip = Ripper.new(code)

pp Ripper.lex code
pp Ripper.tokenize code

# [[[1, 0], :on_ident, "require"],
#  [[1, 7], :on_sp, " "],
#  [[1, 8], :on_tstring_beg, "'"],
#  [[1, 9], :on_tstring_content, "ripper"],
#  [[1, 15], :on_tstring_end, "'"],
#  [[1, 16], :on_nl, "\n"],
#  [[2, 0], :on_ident, "require"],
#  [[2, 7], :on_sp, " "],
#  [[2, 8], :on_tstring_beg, "'"],
#  [[2, 9], :on_tstring_content, "pp"],
#  [[2, 11], :on_tstring_end, "'"],
#  [[2, 12], :on_nl, "\n"],
#  [[3, 0], :on_ignored_nl, "\n"],
#  [[4, 0], :on_ident, "code"],
#  [[4, 4], :on_sp, " "],
#  [[4, 5], :on_op, "="],
#  [[4, 6], :on_sp, " "],
#  [[4, 7], :on_const, "File"],
#  [[4, 11], :on_period, "."],
#  [[4, 12], :on_ident, "read"],
#  [[4, 16], :on_lparen, "("],
#  [[4, 17], :on_kw, "__FILE__"],
#  [[4, 25], :on_rparen, ")"],
#  [[4, 26], :on_nl, "\n"],
#  [[5, 0], :on_ident, "rip"],
#  [[5, 3], :on_sp, " "],
#  [[5, 4], :on_op, "="],
#  [[5, 5], :on_sp, " "],
#  [[5, 6], :on_const, "Ripper"],
#  [[5, 12], :on_period, "."],
#  [[5, 13], :on_ident, "new"],
#  [[5, 16], :on_lparen, "("],
#  [[5, 17], :on_ident, "code"],
#  [[5, 21], :on_rparen, ")"],
#  [[5, 22], :on_nl, "\n"],
#  [[6, 0], :on_ignored_nl, "\n"],
#  [[7, 0], :on_ident, "pp"],
#  [[7, 2], :on_sp, " "],
#  [[7, 3], :on_const, "Ripper"],
#  [[7, 9], :on_period, "."],
#  [[7, 10], :on_ident, "lex"],
#  [[7, 13], :on_sp, " "],
#  [[7, 14], :on_ident, "code"],
#  [[7, 18], :on_nl, "\n"],
#  [[8, 0], :on_ident, "pp"],
#  [[8, 2], :on_sp, " "],
#  [[8, 3], :on_const, "Ripper"],
#  [[8, 9], :on_period, "."],
#  [[8, 10], :on_ident, "tokenize"],
#  [[8, 18], :on_sp, " "],
#  [[8, 19], :on_ident, "code"],
#  [[8, 23], :on_nl, "\n"]]
# ["require",
#  " ",
#  "'",
#  "ripper",
#  "'",
#  "\n",
#  "require",
#  " ",
#  "'",
#  "pp",
#  "'",
#  "\n",
#  "\n",
#  "code",
#  " ",
#  "=",
#  " ",
#  "File",
#  ".",
#  "read",
#  "(",
#  "__FILE__",
#  ")",
#  "\n",
#  "rip",
#  " ",
#  "=",
#  " ",
#  "Ripper",
#  ".",
#  "new",
#  "(",
#  "code",
#  ")",
#  "\n",
#  "\n",
#  "pp",
#  " ",
#  "Ripper",
#  ".",
#  "lex",
#  " ",
#  "code",
#  "\n",
#  "pp",
#  " ",
#  "Ripper",
#  ".",
#  "tokenize",
#  " ",
#  "code",
#  "\n"]
```