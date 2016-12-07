+++
title = "middleman-blogをver4にアップグレード"
date = "2016-01-11T00:00:00+09:00"
tags = ["middleman", "githubpages"]
+++

middleman-blogで書いているブログをver4に上げた、ちょっと面倒だった。

- [Middleman: Upgrading to v4](https://middlemanapp.com/basics/upgrade-v4/)
- [Middleman: v4 へのアップグレード](https://middlemanapp.com/jp/basics/upgrade-v4/)


### 暗黙の拡張子機能(Implied Extension feature)削除に伴うリネーム

- markdownファイルは、明示的に拡張子 .html.md などとしないといけないのでリネーム
- テンプレートは今までも html.hamlのようになっていたのでそれ揃える
- レイアウトは今まで通り htmlはつけない

### Gemfile
- Compass がextensionに取り込まれたので、 `gem "middleman-compass", '>= 4.0.0'` を追加
- アセットパイプラインが廃止されたので `gem "middleman-sprockets", "~> 4.0.0.rc"` を追加
- middleman-blog.gem は [Duplicated key Warning · Issue \#278 · middleman/middleman\-blog](https://github.com/middleman/middleman-blog/issues/278) に引っかかったので githubから。 `gem "middleman-blog", github: "middleman/middleman-blog"`

### config.rb

- partial_dirs の設定が効かないので削除し、テンプレート内で `partial :some_partial` となってる箇所を `partial "partials/some_partial"` と書き換え
- なぜか font_dir になっていたのを fonts_dir に変更
- environment :server, :build :development, :production ブロックを作成しいれられるものはそこに移動

### stylesheets

- 新規に生成してみると stylesheets/内で normalize.css を _normalize.scss として site.css.scss で@importしているので、それに習い、@import される cssをscss、頭にアンダースコアを追加リネーム

### TODO

- アセットパイプラインを外部パイプラインに置き換えていく [Middleman: 外部パイプライン](https://middlemanapp.com/jp/advanced/external-pipeline/)
- hamlをsilmに置き換え
