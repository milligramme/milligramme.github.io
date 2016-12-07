+++
title = "OpenOffice （OSX） でセル内容がダブる"
date = "2014-03-12T00:00:00+09:00"
tags = ["xlsx", "osx"]

outdated = true
+++

OSX 10.8.5 の OpenOffice 4.0.1 で特定の.xlsx を開くとセル内容が何重にもだぶってしまう現象があって調べてみたら

![/images/2014-03-12_duplicated-cells.png](/images/2014-03-12_duplicated-cells.png)

[.xlsx読み込み　セル内の文字が二重、三重に増える (トピック) • OpenOffice.org コミュニティーフォーラム](https://forum.openoffice.org/ja/forum/viewtopic.php?f=10&t=1228)

というのがあって、根本的に解決策がなさそうなので、LibreOffice を入れてデフォルトを設定した

とりあえず、様子見で併用してみることにしたけど、できれば軽いOpenOfficeの方したいところ
  
![/images/2014-03-12_libre4.2.1.1.png](/images/2014-03-12_libre4.2.1.1.png)

![/images/2014-03-12_ooo4.0.1.png](/images/2014-03-12_ooo4.0.1.png)

あと、 QuickLook や Preview.app で開いたときもダブらない