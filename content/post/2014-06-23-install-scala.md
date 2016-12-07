+++
title = "Scalaをhomebrewでインストール"
date = "2014-06-23T00:00:00+09:00"
tags = ["scala", "homebrew"]

outdated = true
+++

業務で使っている [takezoe/gitbucket](https://github.com/takezoe/gitbucket) のさらなる理解と 

jar を生成できる言語を一つ習得しておいた方が今後よさそう、ということで [The Scala Programming Language](http://www.scala-lang.org/) 

homebrew で ver 2.9, 2.10, 2.11 をインストールできる

```
% brew search scala
scala    scala210  scala29
homebrew/science/scalapack
% brew info scala
scala: stable 2.11.1
http://www.scala-lang.org/
Not installed
From: https://github.com/Homebrew/homebrew/commits/master/Library/Formula/scala.rb
==> Options
--with-docs
  Also install library documentation
--with-src
  Also install sources for IDE support
==> Caveats
To use with IntelliJ, set the Scala home to:
  /usr/local/opt/scala/idea
% brew install scala
==> Downloading http://www.scala-lang.org/files/archive/scala-2.11.1.tgz
######################################################################## 100.0%
==> Downloading https://raw.githubusercontent.com/scala/scala-dist/27bc0c25145a83691e3678c7dda602e765e13413/completion.d/2.9.1/scala
######################################################################## 100.0%
==> Caveats
To use with IntelliJ, set the Scala home to:
  /usr/local/opt/scala/idea

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/scala/2.11.1: 45 files, 28M, built in 116 seconds
```

REPL

```
% scala
Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF8
Welcome to Scala version 2.11.1 (Java HotSpot(TM) 64-Bit Server VM, Java 1.6.0_65).
Type in expressions to have them evaluated.
Type :help for more information.

scala> :help
All commands can be abbreviated, e.g. :he instead of :help.
:cp <path>               add a jar or directory to the classpath
:edit <id>|<line>        edit history
:help [command]          print this summary or command-specific help
:history [num]           show the history (optional num is commands to show)
:h? <string>             search the history
:imports [name name ...] show import history, identifying sources of names
:implicits [-v]          show the implicits in scope
:javap <path|class>      disassemble a file or class name
:line <id>|<line>        place line(s) at the end of history
:load <path>             interpret lines in a file
:paste [-raw] [path]     enter paste mode or paste a file
:power                   enable power user mode
:quit                    exit the interpreter
:replay                  reset execution and replay all previous commands
:reset                   reset the repl to its initial state, forgetting all session entries
:save <path>             save replayable session to a file
:sh <command line>       run a shell command (result is implicitly => List[String])
:settings [+|-]<options> +enable/-disable flags, set compiler options
:silent                  disable/enable automatic printing of results
:type [-v] <expr>        display the type of an expression without evaluating it
:kind [-v] <expr>        display the kind of expression's type
:warnings                show the suppressed warnings from the most recent line which had any

scala> println("Hello World")
Hello World

scala> :quit
```