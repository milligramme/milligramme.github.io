+++
title = "vagrant-vbguest"
date = "2014-04-08T00:00:00+09:00"
tags = ["vagrant"]

outdated = true
+++

vagrantで VirtualBox のホストのバージョンとゲストのバージョンをそろえる

`vagrant up` するときにアラートが出る

```
% vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
[default] Clearing any previously set forwarded ports...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Booting VM...
[default] Waiting for machine to boot. This may take a few minutes...
[default] Machine booted and ready!
[default] The guest additions on this VM do not match the installed version of
VirtualBox! In most cases this is fine, but in rare cases it can
prevent things such as shared folders from working properly. If you see
shared folder errors, please make sure the guest additions within the
virtual machine match the version of VirtualBox you have installed on
your host and reload your VM.

Guest Additions Version: 4.2.16
VirtualBox Version: 4.3
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- /vagrant
[default] VM already provisioned. Run `vagrant provision` or use `--provision` to force it
```

[dotless-de/vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) プラグインをいれるとよい

```
% vagrant plugin install vagrant-vbguest
Installing the 'vagrant-vbguest' plugin. This can take a few minutes...
Installed the plugin 'vagrant-vbguest (0.10.0)'!
```



```
% vagrant vbguest
GuestAdditions versions on your host (4.3.10) and guest (4.2.16) do not match.
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: ftp.nara.wide.ad.jp
 * extras: ftp.nara.wide.ad.jp
 * updates: ftp.nara.wide.ad.jp
Setting up Install Process
Package kernel-devel-2.6.32-431.11.2.el6.x86_64 already installed and latest version
Package gcc-4.4.7-4.el6.x86_64 already installed and latest version
Package 1:make-3.81-20.el6.x86_64 already installed and latest version
Package 4:perl-5.10.1-136.el6.x86_64 already installed and latest version
Nothing to do
Copy iso file /Applications/VirtualBox.app/Contents/MacOS/VBoxGuestAdditions.iso into the box /tmp/VBoxGuestAdditions.iso
Installing Virtualbox Guest Additions 4.3.10 - guest version is 4.2.16
Verifying archive integrity... All good.
Uncompressing VirtualBox 4.3.10 Guest Additions for Linux............
VirtualBox Guest Additions installer
Removing installed version 4.2.16 of VirtualBox Guest Additions...
Copying additional installer modules ...
Installing additional modules ...
Removing existing VirtualBox DKMS kernel modules[  OK  ]
Removing existing VirtualBox non-DKMS kernel modules[  OK  ]
Building the VirtualBox Guest Additions kernel modules[  OK  ]
Doing non-kernel setup of the Guest Additions[  OK  ]
You should restart your guest to make sure the new modules are actually used

Installing the Window System drivers
Could not find the X.Org or XFree86 Window System, skipping.
An error occurred during installation of VirtualBox Guest Additions 4.3.10. Some functionality may not work as intended.
In most cases it is OK that the "Window System drivers" installation failed.
```

Window System driversのインストールでエラーが出たけど無視してよさそう

`vagrant up` でアラート出なくなった

```
% vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
[default] Clearing any previously set forwarded ports...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Booting VM...
[default] Waiting for machine to boot. This may take a few minutes...
[default] Machine booted and ready!
GuestAdditions 4.3.10 running --- OK.
[default] Configuring and enabling network interfaces...
[default] Mounting shared folders...
[default] -- /vagrant
[default] VM already provisioned. Run `vagrant provision` or use `--provision` to force it
```