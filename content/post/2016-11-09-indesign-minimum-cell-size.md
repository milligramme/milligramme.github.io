+++
title = "InDesign.jsxで表セルを3pt幅以下にできなくなった"
date = "2016-11-09T09:23:00+09:00"
tags = ["indesign", "extendscript", "table"]

outdated = false
+++

InDesignの表セルの幅は GUIだと3pt以下に設定できない（＝1mmはできない）。

CS5だとスクリプトでその制限が回避できたのが、CCだとアドビがバグ修正できたおかげでできなくなった（CS6から）。

<script src="https://gist.github.com/milligramme/92342e6852ab99d3eab199685c7fa784.js"></script>

高さの方は相変わらず、3pt以下、0まで設定できる。これもそのうちきっとアドビがバグ修正してくれるはず。

<script src="https://gist.github.com/milligramme/3e7e2cbb3a6b0798dab1f915e9bc6a0c.js"></script>

