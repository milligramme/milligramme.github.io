+++
title = "TextMateのBundleSupportの内容"
date = "2014-05-14T00:00:00+09:00"
tags = ["textmate"]
+++

TextMateの Bundle のヘルパーの内容を確認しておくため一覧してみた。

## BundleSupportのパス

```
ENV["TM_SUPPORT_PATH"] == "~/Library/Application Support/TextMate/Managed/Bundles/Bundle Support.tmbundle/Support/shared"
```


```
$ tree ~/Library/Application\ Support/TextMate/Managed/Bundles/Bundle\ Support.tmbundle/Support/shared
.
├── Sounds
│   ├── Harp.wav
│   └── Whistle.wav
├── bin
│   ├── CocoaDialog-license.txt
│   ├── CocoaDialog.app
│   │   └── Contents
│   │       ├── Info.plist
│   │       ├── MacOS
│   │       │   └── CocoaDialog
│   │       ├── PkgInfo
│   │       └── Resources
│   │           ├── COPYING
│   │           ├── Changelog
│   │           ├── Info.plist
│   │           ├── InfoPlist.strings
│   │           ├── Inputbox.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── MainMenu.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   ├── info.nib.orig
│   │           │   ├── objects.nib
│   │           │   └── objects.nib.orig
│   │           ├── Msgbox.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── PopUpButton.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── Progressbar.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── SecureInputbox.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── Textbox.nib
│   │           │   ├── classes.nib
│   │           │   ├── info.nib
│   │           │   └── keyedobjects.nib
│   │           ├── atom.icns
│   │           ├── cocoadialog.icns
│   │           ├── computer.icns
│   │           ├── document.icns
│   │           ├── find.icns
│   │           ├── finder.icns
│   │           ├── firewire.icns
│   │           ├── folder.icns
│   │           ├── gear.icns
│   │           ├── globe.icns
│   │           ├── hazard.icns
│   │           ├── heart.icns
│   │           ├── hourglass.icns
│   │           ├── info.icns
│   │           ├── ipod.icns
│   │           ├── person.icns
│   │           ├── sound.icns
│   │           └── x.icns
│   ├── Markdown-license.txt
│   ├── Markdown.pl
│   ├── SmartyPants-license.txt
│   ├── SmartyPants.pl
│   ├── checknest.rb
│   ├── find_app
│   ├── html_man.sh
│   ├── man2html
│   ├── mate
│   ├── play
│   └── tm_dialog
├── css
│   └── default.css
├── images
│   ├── arrow-down.gif
│   ├── arrow-none.gif
│   └── arrow-up.gif
├── lib
│   ├── README.txt
│   ├── bash_init.sh
│   ├── bluecloth.rb
│   ├── browser.rb
│   ├── codecompletion.rb
│   ├── current_word.rb
│   ├── dialog.py
│   ├── erb_streaming.rb
│   ├── escape.rb
│   ├── exit_codes.rb
│   ├── html.sh
│   ├── io.rb
│   ├── markdown_to_help.rb
│   ├── osx
│   │   ├── keychain.bundle
│   │   └── plist.bundle
│   ├── password.rb
│   ├── plistlib.py
│   ├── progress.rb
│   ├── ruby1.9
│   │   └── add_1.8_features.rb
│   ├── rubypants.rb
│   ├── scriptmate.rb
│   ├── selected_files_tests.rb
│   ├── shelltokenize.rb
│   ├── textmate.rb
│   ├── tm
│   │   ├── detach.rb
│   │   ├── event
│   │   │   ├── NotificationPreferences.nib
│   │   │   │   ├── designable.nib
│   │   │   │   └── keyedobjects.nib
│   │   │   ├── notification.rb
│   │   │   ├── notification_mechanism
│   │   │   │   ├── base.rb
│   │   │   │   ├── growl.rb
│   │   │   │   ├── null.rb
│   │   │   │   └── tooltip.rb
│   │   │   ├── notification_mechanism.rb
│   │   │   ├── notification_preferences.rb
│   │   │   └── scope_selector_scorer.rb
│   │   ├── event.rb
│   │   ├── executor.rb
│   │   ├── htmloutput.rb
│   │   ├── markdown.rb
│   │   ├── process.rb
│   │   ├── require_cmd.rb
│   │   ├── save_current_document.rb
│   │   └── tempfile.rb
│   ├── tm_helpers.py
│   ├── tm_parser.rb
│   ├── ui.rb
│   ├── web_preview.rb
│   ├── webpreview.py
│   └── webpreview.sh
├── nibs
│   ├── ProgressDialog.nib
│   │   ├── classes.nib
│   │   ├── info.nib
│   │   └── keyedobjects.nib
│   ├── RequestItem.nib
│   │   ├── classes.nib
│   │   ├── info.nib
│   │   └── keyedobjects.nib
│   ├── RequestSecureString.nib
│   │   ├── classes.nib
│   │   ├── info.nib
│   │   └── keyedobjects.nib
│   ├── RequestString.nib
│   │   ├── classes.nib
│   │   ├── info.nib
│   │   └── keyedobjects.nib
│   └── SimpleNotificationWindow.nib
│       ├── classes.nib
│       ├── info.nib
│       └── keyedobjects.nib
├── script
│   ├── default.js
│   ├── sortable.js
│   └── webpreview.js
└── version

29 directories, 134 files
```