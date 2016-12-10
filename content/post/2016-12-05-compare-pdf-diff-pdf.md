+++
tags = [
  "diff-pdf",
  "pdf",
  "acrobat"
]
draft = false
outdated = false
title = "diff-pdfでpdf比較"
description = ""
date = "2016-12-05T19:36:29+09:00"

+++

PDFの比較をAcrobatでやろうとすると、250P超だとハングする可能性があると警告される。

{{% img src="/images/2016-12-05-acrobat-over250p.png" %}}


普段使っている `diff-pdf` をつかって試したメモ。（`$ brew install diff-pdf` でインストールできる）

適当に生成した1275Pのpdfが10分くらいで比較された。(1.3 GHz Intel Core i5 8GB mem)

```
% diff-pdf before.pdf after.pdf --output-diff compare.pdf   [16-12-05 14:23 145Mbps]
%                                                           [16-12-05 14:33 145Mbps]
```




