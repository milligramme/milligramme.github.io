+++
title = "brew depstree => brew deps --tree"
date = "2011-11-02T00:00:00+09:00"
tags = ["homebrew"]

outdated = true
+++

Homebrew の依存関係コマンド

$ man brew にも $ brew help にも書いてなくてわすれるからメモ。

依存関係を階層構造で表示してくれる。

2011-11-30追記

brew-depstree コマンドは削除されて

```
brew deps --tree
```

に変わった

[#8829: Move `brew depstree` into `brew deps --tree` - Issues - mxcl/homebrew - GitHub](https://github.com/mxcl/homebrew/issues/8829) 

```
♪ brew depstree ffmpeg
ffmpeg
> yasm
> x264
> > yasm
> faac
> lame
> theora
> > pkg-config
> > libogg
> > libvorbis
> > > pkg-config
> > > libogg
> libvorbis
> > pkg-config
> > libogg
> libogg
> libvpx
> > yasm
> xvid
```

deps コマンドは $ man brew に書いてある

```
#第1レベルの依存、アルファベット順
♪ brew deps --1 ffmpeg
faac
lame
libogg
libvorbis
libvpx
theora
x264
xvid
yasm

#第1レベルの依存、トポロジカル順? 
♪ brew deps --1 -n ffmpeg
yasm
x264
faac
lame
theora
libvorbis
libogg
libvpx
xvid

#全ての依存、トポロジカル順? depstreeと同じっぽい
♪ brew deps -n ffmpeg
yasm
x264
faac
lame
pkg-config
libogg
libvorbis
theora
libvpx
xvid
```

### 参考にした

- [トポロジカルソート - Wikipedia](http://ja.wikipedia.org/wiki/%E3%83%88%E3%83%9D%E3%83%AD%E3%82%B8%E3%82%AB%E3%83%AB%E3%82%BD%E3%83%BC%E3%83%88) 