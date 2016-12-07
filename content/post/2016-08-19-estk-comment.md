+++
title = "ESTKのコメントアウトのチルダ"
date = "2016-08-19T19:42:00+09:00"
tags = ["estk", "extendscript"]

outdated = false 
+++

ExtendScript Toolkitをどうしても使わないといけない環境で、
コメントアウト・アンコメントが cmd + / でできないのが
気になってキーボードショートカットを変更しても、なんかおかしい。

![2016 08 19 Estk Comment](/images/2016-08-19-estk-comment.png)

```js
// comment;

//~ // uncomment; // T_T

comment;

//~ comment out; // T_T

uncomment;
```

`//` は `//~` でさらにコメントアウトされてしまう。

`//` を認識しないのか、アンコメントできない。

ESTK.appのパッケージ内の.xmlとか.jsxをのぞいてみて

`ExtendScript Toolkit.app/Contents/SharedSupport/Required/defs.xml`

```xml
<js>
<braces style="10"/>
<comment>
  <block>//~</block>
  <box>
	<start>/*</start>
	<end>*/</end>
	<middle>*</middle>
  </box>
  <stream>
	<start>/*</start>
	<end>*/</end>
  </stream>
</comment>
```

あたりをいじっても変わらないので、いじりどころが違うよう。

よくみると無駄に含まれてる他の言語も `#~` とか `--~` になっている。

しばらくは手動でやるしかないみたい。
