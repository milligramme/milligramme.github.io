+++
title = "InDesign.jsx 整列と分布"
date = "2014-06-24T00:00:00+09:00"
tags = ["extendscript", "indesign"]
+++

いままでInDesign.jsxでオブジェクトの整列と分布に関してのメソッドが PageItemオブジェクトになかったので、実装されてないもののと思っていて（Ai.jsxの件もありAdobe信用してない）、ちまちまbounds系の計算で処理していた

先日、取引先の方からこのメソッドないんですかねという質問に、がんばって探したら見つけたのでメモ

PageItemのメソッドでなく、Documentのメソッドにあって、CS3から実装されていた..

参考： [Document](http://www15.ocn.ne.jp/~preopen/iddomjs/Document.html)

`align()` と `distribute()` 第一引数に PageItems, 第二引数に 揃えおよび分布のタイプを指定、残りはオプション


### オブジェクトの揃え

    align(
      alignDistributeItems,
      alignOption,
      [alignDistributeBounds]
    )

### オブジェクトの分布

    distribute(
      alignDistributeItems,
      distributeOption,
      [alignDistributeBounds],
      [useDistributeMeasurement],
      [absoluteDistributeMeasurement]
    )

第一引数の PageItems は allPageItems を渡してしまうと配置した画像も含めてしまうので pageItems の方

### オプション

#### AlignOptions

    BOTTOM_EDGES | 下端揃え
    HORIZONTAL_CENTERS | 水平方向中央揃え
    LEFT_EDGES | 左端揃え
    RIGHT_EDGES | 右端揃え
    TOP_EDGES | 上端揃え
    VERTICAL_CENTERS | 垂直方向中央揃え


#### DistributeOptions

    BOTTOM_EDGES | 下端揃え
    HORIZONTAL_CENTERS | 水平方向中央揃え
    HORIZONTAL_SPACE | 水平方向に等間隔に分布
    LEFT_EDGES | 左端揃え
    RIGHT_EDGES | 右端揃え
    TOP_EDGES | 上端揃え
    VERTICAL_CENTERS | 垂直方向中央揃え
    VERTICAL_SPACE | 垂直方向に等間隔に分布


#### AlignDistributeBounds

    ITEM_BOUNDS  |  選択範囲に揃える
    MARGIN_BOUNDS  |  マージンに揃える
    PAGE_BOUNDS  |  ページに揃える
    SPREAD_BOUNDS  |  スプレッドに揃える
    

パネルと照合したらだいたい挙動がわかると思う

![2014 06 24 Align Panel](/images/2014-06-24-align-panel.png)

```js
var doc = app.open(File("~/Desktop/test.indd"), false);
var objs = doc.pageItems;

//// オブジェクトの整列
// 選択範囲にそろえて、下揃え
doc.align(objs, AlignOptions.BOTTOM_EDGES, AlignDistributeBounds.ITEM_BOUNDS);
// マージンにそろえて、下揃え（下マージンに揃う）
doc.align(objs, AlignOptions.BOTTOM_EDGES, AlignDistributeBounds.MARGIN_BOUNDS);


//// オブジェクトの分布
// 選択範囲にそろえて、間隔20mm指定、下揃え
doc.distribute(objs, DistributeOptions.BOTTOM_EDGES, AlignDistributeBounds.ITEM_BOUNDS, true, '20mm');
// マージンにそろえて、間隔20mm指定、下揃え
doc.distribute(objs, DistributeOptions.BOTTOM_EDGES, AlignDistributeBounds.MARGIN_BOUNDS, true, '20mm');


//// 等間隔に分布
// 選択範囲にそろえて、水平方向に等間隔 10mm で分布
doc.distribute(objs, DistributeOptions.HORIZONTAL_SPACE, AlignDistributeBounds.ITEM_BOUNDS, true, '10mm');
// 選択範囲にそろえて、垂直方向に等間隔 10mm で分布
doc.distribute(objs, DistributeOptions.VERTICAL_SPACE, AlignDistributeBounds.ITEM_BOUNDS, true, '10mm');
```