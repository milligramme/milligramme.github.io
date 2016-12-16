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

### AppleScript

```applescript
tell application "Finder"
  -- statements
  activate
  set select_items to selection
  set every_items to every item of select_items
  delete every_items
  display dialog "Move to Trash.."
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

### SASS

sassには対応してない

```sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

### YAML

```yaml
foo:
  bar:
    - 123
    - abc
    - DEF
  buz: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```