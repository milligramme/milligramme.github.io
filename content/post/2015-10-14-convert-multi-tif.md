+++
title = "マルチページtiffの変換"
date = "2015-10-14T00:00:00+09:00"
tags = ["tiff", "cli", "osx", "imagemagick"]
+++

osxではどうやっても開けなかったtiffファイル

```
% ls
１.tif   ２.tif   ３.tif   ４.tif
```

結果的にはそれはマルチページTIFFファイルで、windowsだと標準のビューワーで開ける。
osxではimagemagickで変換して開くことができた。

### convertでひとつのpdfに変換

エラーは古いJPEG圧縮に関するもの

```
% convert *.tif multi-page.pdf
convert: Depreciated and troublesome old-style JPEG compression mode, please convert to new-style JPEG compression and notify vendor of writing software. `OJPEGSetupDecode' @ warning/tiff.c/TIFFWarnings/856.
```

### mogrifyで個別pdfに変換

個別のpdfにするなら、`convert` を回すより `mogrify` を使った方が楽

```bash
% mogrify -format pdf *.tif
```

### 参考にした

- [Tagged Image File Format \- Wikipedia](https://ja.wikipedia.org/wiki/Tagged_Image_File_Format#.E3.83.9E.E3.83.AB.E3.83.81.E3.83.9A.E3.83.BC.E3.82.B8)
- [マルチページTIFFファイルの複数ページを開く方法](http://www.oikawa-sekkei.com/web/design/windows/multi-tiff.html)
