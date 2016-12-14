+++
title = "rubyでAdobe InDesignタグ付きテキスト（UTF-16LE）を書き出す"
date = "2015-09-09T00:00:00+09:00"
tags = ["ruby", "indesign"]
+++

InDesignでタグ付きテキストは UTF-16LE の必要がある

- スタイルが設定済みのinddドキュメントに流し込むなら <DefineXXXStyle:xxx> などの設定の記述は省略してもいい
- `<XXXStyle>`のスタイルグループ付スタイルは `\:` で区切る
- CharStyleは終了点で `<CharStyle:>` 文字スタイル[なし]でリセット

適当なサンプル（冗長書き）

```ruby
ver = 7

# 1行目は最低限必要なヘッダで読み込みするOSと合っている必要がある MAC or WIN
header =<<HEADER_TAG
<UNICODE-MAC>
<Version:#{ver}><FeatureSet:InDesign-Japanese>
HEADER_TAG



content =<<CONTENT
<ParaStyle:STYLE_GROUPNAME\:STYLENAME_A>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor <CharStyle:CYAN>Incididunt ut la<CharStyle:>bore et dolore magna aliqua. 
<ParaStyle:STYLENAME_B>Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
<ParaStyle:STYLENAME_B>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
<ParaStyle:STYLENAME_B>Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
CONTENT

# OR プログラム的に生成した content

# InDesignタグ付きテキストの書出し UTF-16LE
File.open("/path/to/tagged_text.txt", "wb:UTF-16LE:UTF-8") do |file|
  file.print(header + content)
end
```

InDesignで読み込みするときには「グリッドフォーマットの適用」のチェックをはずすこと。