+++
title = "TextMate2 で階層メニュー"
date = "2013-09-20T00:00:00+09:00"
tags = ["textmate"]
+++

2013-09-20現在、TextMate2 の Edit Bundles ... でバンドルのスニペットやコマンドの追加すると、バンドルのルートに保存されます。

階層分けしたくても、サブフォルダにまとめたり、移動したりする機能が GUI でまだないので、バンドルファイル内部の plist を編集する必要があります。

### info.plist を編集

`~/Library/Application\ Support/Avian/Bundles/Jsx.tmbundle/info.plist` を開いて編集する。

デフォルトでは mainMenu というキーがないので作成する。

```xml
<!-- info.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>contactEmailRot13</key>
  <string>zvyyvtenzzr.pp@tznvy.pbz</string>
  <key>contactName</key>
  <string>milligramme</string>
  <key>description</key>
  <string>Execute ExtendScript.</string>

  <!-- この辺にメニューのxmlを挿入する -->

  <key>name</key>
  <string>Jsx-ExtendScript</string>
  <key>uuid</key>
  <string>55A7DFBB-8BF1-4A14-984D-777638B9A5E9</string>
</dict>
</plist>
```

### メニュー部分

あくまでサンプル

```xml
<!-- バンドルの第一階層のメインメニュー -->
<key>mainMenu</key>
<dict>
  <key>items</key>
  <array>
    <!-- サブメニューやスニペット、コマンドなどの UUID -->
    <!-- Run App -->
    <string>28F338BA-2C87-447C-8BE5-93F3FE0EB8C5</string>

    <!-- Open in ESTK -->
    <string>0327735A-F886-46EA-B82C-1F1D1E399BAD</string>

    <string>------------------------------------</string>

    <!-- $.writeln() の UUID-->
    <string>D8BB038B-765E-46E8-80DE-055C7B931FA0</string>
  </array>
  
  
  <!-- メインメニュー以下のすべてのサブメニュー -->
  <key>submenus</key>
  <dict>

    <key>28F338BA-2C87-447C-8BE5-93F3FE0EB8C5</key>
    <dict>
      <key>items</key>
      <array>
        <!-- スニペット、コマンドなどの UUID -->
        <!-- サブメニューをさらに入れ子にもできる -->
        <string>1819B4A9-A7C3-47E3-80DF-4F9D3171F450</string>
        <string>CE096C84-816F-4969-B1BB-E6D873890E33</string>
        <string>F8BDF67F-57A9-4B44-AAEA-A31E0AF2D3CA</string>
        <string>FD6B68C6-2CED-4D8C-8C9B-360C0000A10C</string>
        <string>581CCAE3-1B79-4579-B7A4-93A72F3109FC</string>
      </array>
      
      <!-- サブフォルダ名 -->
      <key>name</key>
      <string>Run App</string>
    </dict>
    
    <key>0327735A-F886-46EA-B82C-1F1D1E399BAD</key>
    <dict>
      <key>items</key>
      <array>
        <string>ECFF4CC3-9793-4A70-97D2-F7127A2B5D67</string>
        <string>0DF36B41-6040-4CBC-AC62-8077B19EE889</string>
        <string>92E41A86-1C60-42BC-9268-9CD06AF7E554</string>
        <string>DA15069A-5ACC-4184-9554-5991B9A12272</string>
        <string>832C82CB-763A-4ECD-A9A1-6B7F28B2CABC</string>
      </array>
      <key>name</key>
      <string>Open in ESTK</string>
    </dict>

  </dict>
</dict>
```

### $_writeln().tmSnippet

これらコマンドやスニペットの `uuid` の値を地道に `info.plist` にコピペしてメニューをつくりあげるわけです。

```xml
<!-- $_writeln().tmSnippet -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>content</key>
  <string>\$.writeln(${0});</string>
  <key>name</key>
  <string>$.writeln()</string>
  <key>scope</key>
  <string>source.js</string>
  <key>tabTrigger</key>
  <string>log</string>
  <key>uuid</key>
  <string>D8BB038B-765E-46E8-80DE-055C7B931FA0</string>
</dict>
</plist>
```

### おまけ

コマンドやスニペットを複製してちょっと変えたいとき、GUI で複製ってメニューなどがなくできないけど

```bash
$ cp ./Snippets/xx.tmSnippet ./Snippets/yy.tmSnippet
```

などと複製して、 uuid を別に割り当てあげれば楽にできる。