+++
title = "JSONをparseする時に symboize_namesオプションを使う"
date = "2016-12-09T09:47:03+09:00"
description = ""
tags = ["ruby","json"
]
draft = false
outdated = false

+++

jsonをパースする時 symbolize_namesオプションをつけると、キーがシンボルになる

```rb
require 'json'
require 'pp'

str = DATA.read

h_str = JSON.parse(str)
pp h_str
puts
pp h_str["document_info"]
pp h_str[:document_info] #=> nil
puts
puts
h_sym = JSON.parse(str, symbolize_names: true)
pp h_sym
puts
pp h_sym["document_info"] #=> nil
pp h_sym[:document_info]

__END__
{
  "document_info" : {
    "name":"test.txt",
    "encodign":"utf-8",
    "linefeed":"unix"
  },
  "data":[
    {"line_no": 1, "content":"Lorem ipsum"},
    {"line_no": 2, "content":"sed do eiusmod"},
    {"line_no": 3, "content":"Ut enim veniam"},
    {"line_no": 4, "content":"exercitation ullamco"},
    {"line_no": 5, "content":"Duis aute nulla"}
  ]
}

```

出力

```rb
{"document_info"=>
  {"name"=>"test.txt", "encodign"=>"utf-8", "linefeed"=>"unix"},
 "data"=>
  [{"line_no"=>1, "content"=>"Lorem ipsum"},
   {"line_no"=>2, "content"=>"sed do eiusmod"},
   {"line_no"=>3, "content"=>"Ut enim veniam"},
   {"line_no"=>4, "content"=>"exercitation ullamco"},
   {"line_no"=>5, "content"=>"Duis aute nulla"}]}

{"name"=>"test.txt", "encodign"=>"utf-8", "linefeed"=>"unix"}
nil


{:document_info=>{:name=>"test.txt", :encodign=>"utf-8", :linefeed=>"unix"},
 :data=>
  [{:line_no=>1, :content=>"Lorem ipsum"},
   {:line_no=>2, :content=>"sed do eiusmod"},
   {:line_no=>3, :content=>"Ut enim veniam"},
   {:line_no=>4, :content=>"exercitation ullamco"},
   {:line_no=>5, :content=>"Duis aute nulla"}]}

nil
{:name=>"test.txt", :encodign=>"utf-8", :linefeed=>"unix"}
```


[parse \(JSON\) \- APIdock](http://apidock.com/ruby/JSON/parse)