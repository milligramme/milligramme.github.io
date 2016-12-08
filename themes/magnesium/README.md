# Magnesium

[Hugo](//gohugo.io) theme

##

- PureCSS based
- FontAwesome


## Installation

```
$ cd themes
$ git clone https://github.com/milligramme/magnesium.git
```

## Sample Configuration

```toml
baseurl = "http://replace-this-with-your-hugo-site.com/"
title = "Your site title"
author = "Your name"
theme = "magnesium"

copyright = "<i class='fa fa-copyright fa-fw'></i> 2016. All rights reserved."
canonifyurls = true
paginate = 10

# for CJK lang
hasCJKLanguage = true

[indexes]
  tag = "tags"

[params]
  author = "Your name"
  subtitle = "Subtitle"
  brand = "magnesium"
  googleAnalytics = "Your Google Analytics tracking ID"

  # CSS name for highlight.js https://highlightjs.org/static/demo/
  highlightjs = "atom-one-light"
  
  dateFormat = "2006-01-02 15:04"
  
  # Set local working dir to edit with TextMate
  # This work under Development Mode
  workingdir = "/path/to/hugo/content"

[menu]
  # Shown in the side menu.
  [[menu.main]]
    name = "Posts"
    pre = "<i class='fa fa-list fa-fw'></i>"
    weight = 1
    identifier = "post"
    url = "/post/"
  [[menu.main]]
    name = "Tags"
    pre = "<i class='fa fa-tags fa-fw'></i>"
    weight = 1
    identifier = "tags"
    url = "/tags/"
  [[menu.main]]
    name = "About"
    pre = "<i class='fa fa-user fa-fw'></i>"
    weight = 2
    identifier = "about"
    url = "/about/"

[social]
  twitter = "milligramme"
  facebook = ""
  googleplus = ""

  tumblr = "milligramme"
  pinterest = "milligramme"

  stackoverflow = "1536789"
  github = "milligramme"
  bitbucket = "milligramme"
  gitlab = "milligramme"

  instagram = "milligramme"
  flickr = "milligramme"

  linkedin = ""
  vimeo = "milligramme"
  soundcloud = "milligramme"
  lastfm = "milliqramme"
  youtube = ""
  pinboard = "milligramme"
  slideshare = "GoKoide"
  speakerdeck = "milligramme"
  

```


## Demo

```
$ cd themes/magnesium/exampleSite
$ hugo server -D -w -t=../..
```

# Shortcodes

## Image

```
{{% img src="images/image.jpg" %}}
{{% img src="images/image.jpg" class="right" %}}
{{% img src="images/image.jpg" class="left" %}}
{{% img src="images/image.jpg" caption="Referenced from wikipedia." href="https://en.wikipedia.org/wiki/Lorem_ipsum" %}}
```

## Development mode

Supported development mode.

```
env HUGO_ENV="DEV" hugo server --watch --buildDrafts=true --buildFuture=true --theme magnesium
```

This mode is

* Not show Google Analytics tags.
* Show `IsDraft`.
* Show `WordCount`.

And set `{{ if ne (getenv "HUGO_ENV") "DEV" }} Set elements here. {{ end }}` if you want to place only in a production environment.