---
layout: page
title: create_bootable_ubuntu_on_osx
math_support: mathjax
---


refer to [How to create a bootable USB stick on OS X](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx)

1. download ubuntu iso
2. `hdiutil convert -format UDRW -o ~/path/to/target.img ~/path/to/ubuntu.iso`
3. `diskutil list`
4. insert usb disk
5. `diskutil list` again
6. `diskutil unmountDisk /dev/diskN`
7. `sudo dd if=/path/to/downloaded.img of=/dev/diskN bs=1m` overwrite usb disk
8. `diskutil eject /dev/diskN`


