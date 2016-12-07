+++
title = "middleman build で除外設定"
date = "2016-02-18T00:00:00+09:00"
tags = ["middleman", "webpack"]
+++

Middlemanの外部パイプラインで余分なファイルがbuildされるので調べた

[Exclude files from build? · Issue \#1102 · middleman/middleman](https://github.com/middleman/middleman/issues/1102)

`app.js`, `hello.js`, `bye.js` で `bundle.js` を生成したい

```js
// webpack.config.js
module.exports = {
  context: __dirname + '/assets/javascripts',
  entry: {
    "bundle": ["./app.js", "./hello.js", "./bye.js"]
  },
  
  output: {
    path: __dirname + '/source/javascripts',
    filename: "[name].js"
  }
}
```

```rb
# config.rb
npm_prefix = `npm bin`.strip

activate :external_pipeline, 
  name: "webpack",
  command: build? ? "#{npm_prefix}/webpack --bail" : "#{npm_prefix}/webpack --watch -d",
  source: "./assets/javascripts",
  latency: 1
```

このような設定で `middleman build` すると

       create  build/app.js
       create  build/bye.js
       create  build/hello.js

のように bundle.jsのsrcのjsもが生成されるので、 configure :build ブロックで生成されないよう ignore を追加する

```rb
# config.rb
configure :build do
  # これは効かない
  # ignore 'assets/javascripts/*js'

  Dir.glob "./assets/javascripts/*js" do |js|
    ignore File.basename(js)
  end
end
```

