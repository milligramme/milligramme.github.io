+++
title = "diffコマンドで ＋− 表記"
date = "2015-09-03T00:00:00+09:00"
tags = ["diff", "cli"]
+++

diff コマンドの比較 デフォルトだと `<` と `>` で旧新を表現している

`git diff` のときみたいに `-` と `+` にしたい

```
% diff OLDFILE NEWFILE
```

```diff
% diff  OLD.txt NEW.txt
10,15c10,15
<  68.9999999997305,
<  94.4999999999361],
<  visibleBounds:[20,
<  20,
<  68.9999999997305,
<  94.4999999999361],
---
>  90,
>  89.2499999999361],
>  visibleBounds:[19.5,
>  19.5,
>  90.5,
>  89.7499999999361],
32c32
<  strokeWeight:0,
---
>  strokeWeight:1,
39c39
<  strokeColor:resolve("/document[@id=2]//swatch[@id=14]"),
---
>  strokeColor:resolve("/document[@id=2]//color[@id=11]"),
48a49
>  overprintStroke:false,
```

`-u` オプションをつけて unified出力形式にしたらいい（変更箇所の前後3行が含まれる）

`-U <num>` とすることで、前後に表示する行数を制御できる

```
% diff -U 2 OLDFILE NEWFILE
```

```diff
% diff -U 2 OLD.txt NEW.txt
--- OLD.txt	2015-09-03 19:47:27.000000000 +0900
+++ NEW.txt	2015-09-03 19:48:04.000000000 +0900
@@ -8,10 +8,10 @@
  geometricBounds:[20,
  20,
- 68.9999999997305,
- 94.4999999999361],
- visibleBounds:[20,
- 20,
- 68.9999999997305,
- 94.4999999999361],
+ 90,
+ 89.2499999999361],
+ visibleBounds:[19.5,
+ 19.5,
+ 90.5,
+ 89.7499999999361],
  parentStory:resolve("/document[@id=2]//story[@id=237]"),
  startTextFrame:resolve("/document[@id=2]//text-frame[@id=255]"),
@@ -30,5 +30,5 @@
  fillColor:resolve("/document[@id=2]//swatch[@id=14]"),
  fillTint:-1,
- strokeWeight:0,
+ strokeWeight:1,
  miterLimit:4,
  endCap:({}),
@@ -37,5 +37,5 @@
  leftLineEnd:({}),
  rightLineEnd:({}),
- strokeColor:resolve("/document[@id=2]//swatch[@id=14]"),
+ strokeColor:resolve("/document[@id=2]//color[@id=11]"),
  strokeTint:-1,
  gradientFillStart:[104.999999999968,
@@ -47,4 +47,5 @@
  gradientStrokeLength:0,
  gradientStrokeAngle:0,
+ overprintStroke:false,
  gapColor:resolve("/document[@id=2]//swatch[@id=14]"),
  gapTint:-1,
```

### 参考にした

```
% diff --help
Usage: diff [OPTION]... FILES
Compare files line by line.

  -i  --ignore-case  Ignore case differences in file contents.
  --ignore-file-name-case  Ignore case when comparing file names.
  --no-ignore-file-name-case  Consider case when comparing file names.
  -E  --ignore-tab-expansion  Ignore changes due to tab expansion.
  -b  --ignore-space-change  Ignore changes in the amount of white space.
  -w  --ignore-all-space  Ignore all white space.
  -B  --ignore-blank-lines  Ignore changes whose lines are all blank.
  -I RE  --ignore-matching-lines=RE  Ignore changes whose lines all match RE.
  --strip-trailing-cr  Strip trailing carriage return on input.
  -a  --text  Treat all files as text.

  -c  -C NUM  --context[=NUM]  Output NUM (default 3) lines of copied context.
  -u  -U NUM  --unified[=NUM]  Output NUM (default 3) lines of unified context.
    --label LABEL  Use LABEL instead of file name.
    -p  --show-c-function  Show which C function each change is in.
    -F RE  --show-function-line=RE  Show the most recent line matching RE.
  -q  --brief  Output only whether files differ.
  -e  --ed  Output an ed script.
  --normal  Output a normal diff.
  -n  --rcs  Output an RCS format diff.
  -y  --side-by-side  Output in two columns.
    -W NUM  --width=NUM  Output at most NUM (default 130) print columns.
    --left-column  Output only the left column of common lines.
    --suppress-common-lines  Do not output common lines.
...
```