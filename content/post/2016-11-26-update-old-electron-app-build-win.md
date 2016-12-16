+++
tags = ["electron"
]
date = "2016-11-26T17:17:31+09:00"
draft = false
outdated = false
title = "Windows用Electron.appをMacでビルド"
description = ""

+++

古い環境でつくったElectron.appをアップグレードした。

- nodejs v4.4.5 -> v6.3.0
- electron v0.36.7 -> v1.4.8


その後、 [electron\-userland/electron\-packager](https://github.com/electron-userland/electron-packager) でのappの生成でmacからwinのアプリが生成できなくなった。


build.sh ビルドスクリプト

```bash
#!/usr/bin/env bash
electron_version=1.4.8
build_version=`date '+%F'`
app_version="0.6.0"

echo "Build for OSX.."
electron-packager electron-app build \
  --platform=darwin \
  --arch=x64 \
  --overwrite \
  --version=$electron_version \
  --app-version=$app_version \
  --build-version=$build_version
echo
echo "Build for  WIN 64bit"
electron-packager electron-app build \
  --platform=win32 \
  --arch=x64 \
  --overwrite \
  --version=$electron_version \
  --build-version=$build_version \
  --app-version=$app_version
echo
echo "Build for  WIN 32bit"
electron-packager electron-app build \
  --platform=win32 \
  --arch=ia32 \
  --overwrite \
  --version=$electron_version \
  --build-version=$build_version \
  --app-version=$app_version
```

Windows用のビルドに失敗

```
% ./build.sh
Build for OSX..
Packaging app for platform darwin x64 using electron v1.4.8
Wrote new app to /Users/gdansk/work/electron-darwin-x64

Build for  WIN 64bit
Downloading electron-v1.4.8-win32-x64.zip
[============================================>] 100.0% of 54.33 MB (2.22 MB/s)
Packaging app for platform win32 x64 using electron v1.4.8
Could not find "wine" on your system.

Wine is required to use the app-copyright, app-version, build-version, icon, and
win32metadata parameters for Windows targets.

Make sure that the "wine" executable is in your PATH.

See https://github.com/electron-userland/electron-packager#building-windows-apps-from-non-windows-platforms for details.

Build for  WIN 32bit
Downloading electron-v1.4.8-win32-ia32.zip
[============================================>] 100.0% of 44.02 MB (1.66 MB/s)
Packaging app for platform win32 ia32 using electron v1.4.8
Could not find "wine" on your system.

Wine is required to use the app-copyright, app-version, build-version, icon, and
win32metadata parameters for Windows targets.

Make sure that the "wine" executable is in your PATH.

See https://github.com/electron-userland/electron-packager#building-windows-apps-from-non-windows-platforms for details.
```


[electron\-userland/electron\-packager: Package and distribute your Electron app with OS\-specific bundles \(\.app, \.exe etc\) via JS or CLI](https://github.com/electron-userland/electron-packager#building-windows-apps-from-non-windows-platforms)


wine1.6以上が必要なので homebrewでインストール

```
% brew search wine
wine                                                   winetricks                                             winexe
Caskroom/cask/twine                                                               Caskroom/cask/wineskin-winery

% brew install wine
==> Installing dependencies for wine: libpng, freetype, jpeg, libtool, libusb, libusb-compat, fontconfig, libtiff, webp, gd, libgphoto2, little-cms2, jasper, libicns, makedepend, openssl, net-snmp, sane-backends, libtasn1, gmp, nettle, gnutls

...
... ものすごい依存
...

==> Installing wine
==> Downloading https://homebrew.bintray.com/bottles/wine-1.8.5.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring wine-1.8.5.yosemite.bottle.tar.gz
==> Caveats
You may want to get winetricks:
  brew install winetricks

By default Wine uses a native Mac driver. To switch to the X11 driver, use
regedit to set the "graphics" key under "HKCUSoftwareWineDrivers" to
"x11" (or use winetricks).

For best results with X11, install the latest version of XQuartz:
  https://xquartz.macosforge.org/
==> Summary
🍩  /usr/local/Cellar/wine/1.8.5: 2,507 files, 260.9M
```


リトライするが失敗

```
% ./build.sh
Build for OSX..
Packaging app for platform darwin x64 using electron v1.4.8
Wrote new app to /Users/gdansk/work/build/electron-app-darwin-x64

Build for  WIN 64bit
Packaging app for platform win32 x64 using electron v1.4.8
rcedit.exe failed with exit code 1. Fatal error: Unable to parse version string

Build for  WIN 32bit
Packaging app for platform win32 ia32 using electron v1.4.8
rcedit.exe failed with exit code 1. Fatal error: Unable to parse version string
```

オプションをデフォルトにして、`--all` ですべてビルドはできる

```
% electron-packager ./ --version=1.4.8 --all
```


[Build version should default to app version on Windows · Issue \#498 · electron\-userland/electron\-packager](https://github.com/electron-userland/electron-packager/issues/498)

Windows用では `--app-version=$app_version` と `--build-version=$app_version` を指定しないとビルドできた


```
% ./build.sh
Build for OSX..
Packaging app for platform darwin x64 using electron v1.4.8
Wrote new app to /Users/gdansk/work/build/electron-app-darwin-x64

Build for  WIN 64bit
Packaging app for platform win32 x64 using electron v1.4.8
Wrote new app to /Users/gdansk/work/build/electron-app-win32-x64

Build for  WIN 32bit
Packaging app for platform win32 ia32 using electron v1.4.8
Wrote new app to /Users/gdansk/work/build/electron-app-win32-ia32
```

