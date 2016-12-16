+++
description = ""
tags = [
"javascript","hugo"
]
categories = [
]
draft = false
outdated = false
date = "2016-12-16T11:09:31+09:00"
title = "Hugo syntaxhighlight test"

+++

CDN経由の highlight.js でよみこまれない言語のうち以下を追加

### Golang

```go
package main

import "fmt"
func main() {
    fmt.Println("hello world")
}
```

### AppleScript

```applescript
tell application "Finder"
  -- hello
  activate
  display dialog "Hello World."
end tell
```

### Processing

test.pde

```processing
import processing.video.*;

Capture video;
void setup() {
  size(600, 480);
  video = new Capture(this, 640, 480, 30);
  video.start();
}
void captureEvent(Capture video) {
  video.read();
}
void draw () {
  background(0);
  image(video, random(width), 0, random(width), random(height));
}
```

### HAML

```haml
%section.container
  %h1= post.title
  %h2= post.subtitle
  .content
    = post.content
```

### SCSS

```scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

sassには対応してない

```scss
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

### YAML

```yaml
invoice: 34843
date   : 2001-01-23
bill-to: &id001
    given  : Chris
    family : Dumars
    address:
        lines: |
            458 Walkman Dr.
            Suite #292
        city    : Royal Oak
        state   : MI
        postal  : 48046
ship-to: *id001
product:
    - sku         : BL394D
      quantity    : 4
      description : Basketball
      price       : 450.00
    - sku         : BL4438H
      quantity    : 1
      description : Super Hoop
      price       : 2392.00
tax  : 251.42
total: 4443.52
comments: >
    Late afternoon is best.
    Backup contact is Nancy
    Billsmer @ 338-4338.
```