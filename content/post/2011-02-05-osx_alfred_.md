+++
title = "理想のランチャーを探す Alfred ?"
date = "2011-02-05T00:00:00+09:00"
tags = ["alfred", "indesign", "osx"]
+++
フィヨルドの @komagata さんのエントリー「 [理想のランチャーを探す - komagata [p0t]](http://docs.komagata.org/4723) 」をみてて
「ランチャーから自分のブログ内を検索」というコマンドのことがあって、自分もそういう使い方する！（InDesignのExtendScript書くときにpropertyとかmethodとかよく忘れる）ので、自分が使っているランチャー Alfred（Free版）についてあらためて調べてみました。（最近 @soundflower さんに教えてもらい  [Namely](http://amarsagoo.info/namely/)  から乗り換えました）

[Alfred](http://www.alfredapp.com/)

![/images/2011/02/alfred_home.png](/images/2011/02/alfred_home.png)

サイト内検索は Custom Search に追加してあげることで、簡単にできました。
![/images/2011/02/alfred_custom_search_field.png](/images/2011/02/alfred_custom_search_field.png)

  - ブラウザで、何かキーワードをサイト内検索してみる
  - urlをコピー、**Search URL:**にペースト
  - 先ほどの検索キーワードを「{query}」と書き換える
  - **Display text:** に表示名を入力
  - **Keyword: **に検索キーとなる文字を入力、左はTEST用のクエリ欄
  - お好みで検索クエリのUTF-8エンコーディングにチェック
  - SAVE

これでランチャーで　「mg もじゃもじゃ」と入力すると
 [http://www.milligramme.cc/wp/?s=もじゃもじゃ](http://www.milligramme.cc/wp/?s=%E3%82%82%E3%81%98%E3%82%83%E3%82%82%E3%81%98%E3%82%83) 
とサイト内検索されます。

InDesignでスクリプトを書いていて、サイト内検索でお世話になっているといえば、「 [お〜まちさんの InDesign Object Model](http://www15.ocn.ne.jp/~preopen/dom_about.html) 」です。こちらのGoogleサイト内検索の結果を加工して、追加してみると、

![/images/2011/02/alfred_custom_search.png](/images/2011/02/alfred_custom_search.png)

ランチャーから InDesignのObject Modelの検索ができます。便利。ESTKいらない！

ほかに、プリセットのWeb Searchとして、普通のGoogle検索、画像検索などなど、GMail、GoogleReader起動、Twitter検索、Youtube検索とかもあるので、これらもランチャーから実行できます。（多分高機能ランチャーなら普通にできるのだと思います）

また、自分はまだ Powerpack版にしてないのですが、アップグレードするとMail送信？、クリップボード管理？、iTunesのコントロール？、Terminalの操作？ができるようです。

Terminal操作がどんなものかちょっと気になるので、使っている方がいたら教えてもらいたいところです。

Free版が物足りなくなったらPowerpack版検討してみようかと思います。

支払い方法にPayPalがまだ対応中ということで、現在はGoogleCheckOutのみで £12 (2010-02-05現在 1,590円くらい)。

### 参考にした

- [Become an Alfred Expert: Advanced Tips &amp; Tricks \| Mac.AppStorm](http://mac.appstorm.net/how-to/utilities-how-to/become-an-alfred-expert-advanced-tips-tricks/)  :Tips
- [Community-powered support for Alfred App](http://getsatisfaction.com/alfredapp)  :Alfred コミュニティ
  



### p.s.
Preferences/Experimental に「押してはいけない」ボタンというのがあってとても気になります。遊び心あって期待ができます。

![/images/2011/02/alfred_dontpress.png](/images/2011/02/alfred_dontpress.png)