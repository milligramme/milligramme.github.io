+++
title = "bundle viz コマンド w/GraphViz"
date = "2011-12-13T00:00:00+09:00"
tags = ["bundler"]
+++
bundle viz コマンドをためしてみる

```
gdansk: ~/Project/Sakiika master ♪ bundle viz
#<LoadError: cannot load such file -- graphviz>
Make sure you have the graphviz ruby gem. You can install it with:
`gem install ruby-graphviz`
```

ruby-graphviz がないのでインストール

```
gdansk: ~/Project/Sakiika master ♪ gem install ruby-graphviz
Fetching: ruby-graphviz-1.0.0.gem (100%)

Since version 0.9.2, Ruby/GraphViz can use Open3.popen3 (or not)
On Windows, you can install 'win32-open3'

You need to install GraphViz (http://graphviz.org/) to use this Gem.

For more information about Ruby-Graphviz :
* Doc : http://rdoc.info/projects/glejeune/Ruby-Graphviz
* Sources : http://github.com/glejeune/Ruby-Graphviz
* NEW - Mailing List : http://groups.google.com/group/ruby-graphviz

/!\ Version 0.9.12 introduce a new solution to connect edges to node ports
For more information, see http://github.com/glejeune/Ruby-Graphviz/issues/#issue/13
So if you use node ports, maybe you need to change your code.

/!\ GraphViz::Node#name has been removed!

/!\ :output and :file options have been removed!

/!\ The html attribut has been removed!
You can use the label attribut, as dot do it : :label => '<<html/>>'

/!\ Version 0.9.17 introduce GraphML (http://graphml.graphdrawing.org/) support and
graph theory !
Successfully installed ruby-graphviz-1.0.0
1 gem installed
Installing ri documentation for ruby-graphviz-1.0.0...
Installing RDoc documentation for ruby-graphviz-1.0.0...
```

GraphViz をインストール

[Download \| Graphviz - Graph Visualization Software](http://www.graphviz.org/Download.php) 

```
gdansk: ~/Project/Sakiika master ♪ bundle viz
/Users/gdansk/.rvm/gems/ruby-1.9.3-p0/gems/ruby-graphviz-1.0.0/lib/graphviz/utils.rb:26: Use RbConfig instead of obsolete and deprecated Config.
/Users/gdansk/Project/Sakiika/gem_graph.png
```

プロジェクトのルートで $ bundle viz を実行すると、gem_graph.png を出力、gemの依存関係をビジュアライズドしてくれる

![/images/2011/12/gem_graph.png](/images/2011/12/gem_graph.png)