+++
title = "plistをrubyとかcliでいじる"
date = "2013-10-04T00:00:00+09:00"
tags = ["ruby", "plist", "cli"]
+++

TextMate のバンドルの編集をもっと楽にできないかと plist のいじり方を調べた。

### plist.gem を使う

Plist::parse_xml([xmlまたはファイルのパス]) で指定

```ruby
require "plist"
require "json"

xml_path = "~/Library/Application Support/Avian/Bundles/Jsx.tmBundle/info.plist"
hash = Plist::parse_xml(File.expand_path xml_path)
puts JSON.pretty_generate(hash)

# {
#   "contactEmailRot13": "zvyyvtenzzr.pp@tznvy.pbz",
#   "contactName": "milligramme",
#   "description": "Execute ExtendScript.",
#   "mainMenu": {
#     "items": [
#       "28F338BA-2C87-447C-8BE5-93F3FE0EB8C5",
#       "0327735A-F886-46EA-B82C-1F1D1E399BAD",
#       "------------------------------------",
#       "86F17501-1D4D-4FD8-A8E5-4D53A57CF346",
#       "A83E4850-BC62-4207-804D-F30B6EC78BC4",
#       "------------------------------------",
#       "426AE9AD-D61D-48BB-9B2C-FC8832339F4E",
#       "B4EEA808-F06C-40FE-88F0-3BDCA8E5C838",
#       "0D256A62-AE52-4122-984E-1E243318C557",
#       "EA646123-711E-4667-BA8A-54C001419FC8",
#       "------------------------------------",
#       "D8BB038B-765E-46E8-80DE-055C7B931FA0",
#       "CB511578-C309-4A68-8901-ACCFCFD07102"
#     ],
#     "submenus": {
#       "0327735A-F886-46EA-B82C-1F1D1E399BAD": {
#         "items": [
#           "ECFF4CC3-9793-4A70-97D2-F7127A2B5D67",
#           "0DF36B41-6040-4CBC-AC62-8077B19EE889",
#           "92E41A86-1C60-42BC-9268-9CD06AF7E554",
#           "DA15069A-5ACC-4184-9554-5991B9A12272",
#           "832C82CB-763A-4ECD-A9A1-6B7F28B2CABC"
#         ],
#         "name": "Open in ESTK"
#       },
#       "0D256A62-AE52-4122-984E-1E243318C557": {
#         "items": [
#           "------------------------------------"
#         ],
#         "name": "Photoshop"
#       },
#       "28F338BA-2C87-447C-8BE5-93F3FE0EB8C5": {
#         "items": [
#           "1819B4A9-A7C3-47E3-80DF-4F9D3171F450",
#           "CE096C84-816F-4969-B1BB-E6D873890E33",
#           "F8BDF67F-57A9-4B44-AAEA-A31E0AF2D3CA",
#           "FD6B68C6-2CED-4D8C-8C9B-360C0000A10C",
#           "581CCAE3-1B79-4579-B7A4-93A72F3109FC"
#         ],
#         "name": "Run App"
#       },
#       "426AE9AD-D61D-48BB-9B2C-FC8832339F4E": {
#         "items": [
#           "CB6C2AC2-E1F5-4381-9D99-6CD18D890E89",
#           "C6DF606C-F21F-487D-8ED4-2CE51F0DFACD",
#           "950D23F2-D1C4-4569-8224-97516426E7BF",
#           "DE9EE989-74B9-4EFF-AC07-72A7134CB4A0",
#           "4156C4E2-81CF-4431-B1A6-7F8D2FFE5948",
#           "------------------------------------",
#           "93BD9450-64F5-4DAA-9316-0A2B7561EAA0",
#           "765E4264-5217-44B8-820E-0A823F11B724",
#           "AF537D72-1792-4579-B3B2-7314F1788B81",
#           "1B02C88C-1EFE-4EF2-B52A-1A9E5A2E0A5A"
#         ],
#         "name": "InDesign"
#       },
#       "86F17501-1D4D-4FD8-A8E5-4D53A57CF346": {
#         "items": [
#           "9244867A-2D4B-4999-85E5-C4E7B4368D6B",
#           "D0481FDF-A50B-491D-AA01-58FFAA7544DD",
#           "507BEA91-C19A-49A1-B138-39D5AD83236D",
#           "39C8ED35-F19E-4BE2-B7D7-DAC05E73D19A",
#           "------------------------------------",
#           "247BACF0-1512-4952-BE51-D3DE60AC6BC4",
#           "3D714804-4016-4067-B323-4666C4CAB689"
#         ],
#         "name": "Header"
#       },
#       "950D23F2-D1C4-4569-8224-97516426E7BF": {
#         "items": [
#           "BCB3D594-BD1B-462D-92E7-062FCA8D33D9",
#           "6DF1EE1C-66E6-40BE-B0AF-E7F1EB9449ED",
#           "546EFF8D-A616-402E-9AE6-EAB6BDAD0315",
#           "F67E11B4-8A7C-45C8-9C71-C9F26D67102A"
#         ],
#         "name": "Find and Change"
#       },
#       "A83E4850-BC62-4207-804D-F30B6EC78BC4": {
#         "items": [
#           "9DDAFB45-3B46-45C8-BA8F-83B496182DB7",
#           "------------------------------------",
#           "063C6236-CF8F-4A05-B4AC-C0F23D172C09",
#           "42D54CC7-2926-4A66-85BB-A7D23EFCC7F2",
#           "42BD5882-03D8-4C0B-9E8E-CC384567298D",
#           "------------------------------------",
#           "E57F5CA7-45B9-48DC-83B3-0001CD4F3216",
#           "D0146FB5-FC5E-4069-A557-83C5D2D72BE0",
#           "B4D97E21-1D22-413E-B077-3FDC6069683B",
#           "B1CB5A2D-04B9-4AEA-8375-DEDFFBE549E3",
#           "B9C3252E-2CA2-4D5F-B38A-10E3FE7ED917"
#         ],
#         "name": "Core"
#       },
#       "B4EEA808-F06C-40FE-88F0-3BDCA8E5C838": {
#         "items": [
#           "2B10796C-88DF-45DC-AB61-0754792B9C8B"
#         ],
#         "name": "Illustrator"
#       },
#       "DE9EE989-74B9-4EFF-AC07-72A7134CB4A0": {
#         "items": [
#           "E0FD459C-5BF4-4BDC-B751-048875AFB828",
#           "8BD1BFCB-B040-4B7E-BA01-9F1B3178CACD",
#           "87DF3F66-8F79-46BE-B0C0-88CD7C7B1582"
#         ],
#         "name": "Interaction Level"
#       },
#       "EA646123-711E-4667-BA8A-54C001419FC8": {
#         "items": [
#           "E6FCE63B-8CDE-4BA7-96B2-5CD7F3DC8972"
#         ],
#         "name": "ScriptUI"
#       }
#     }
#   },
#   "name": "Jsx-ExtendScript",
#   "ordering": [
#     "------------------------------------"
#   ],
#   "uuid": "55A7DFBB-8BF1-4A14-984D-777638B9A5E9"
# }


bin_path = "~/Library/Application Support/TextMate/Managed/Bundles/Text.tmbundle/info.plist"
begin
  bhash = Plist::parse_xml(File.expand_path bin_path)
  puts JSON.pretty_generate(bhash)
rescue Exception => e
  puts e
end
# エラー　バイナリーplistはみられない
# => invalid byte sequence in UTF-8
```


### plutil コマンドで plist を変換

`plutil` コマンドで xml形式plist・バイナリー形式plist、json の相互変換ができる

`-convert xml1` オプションで xml形式に、`-convert binary1` でバイナリー形式に変換する、直接変換（上書き保存）する。`-o path` オプションで別名保存できる。

ついでに `-convert json` で jsonにしてくれる、`-r -convert json` でprettyなjsonにしてくれる。

```bash
$ which plutil
/usr/bin/plutil
$ plutil -convert xml1 ~/Library/Application\ Support/TextMate/Managed/Bundles/Text.tmbundle/info.plist -o ~/Desktop/info.plist
```