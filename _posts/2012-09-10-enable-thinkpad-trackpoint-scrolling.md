---
layout: post
title:  Enable thinkpad trackpoint scrolling on RHEL6/7
date:   2012-09-10 13:41:10 +0800
---

### Use `xinput` command to find the trackpoint device name

```shell
$ xinput
```

### Create file `/etc/X11/xorg.conf.d/10-thinkpad-trackpad-scroll.conf` with following content.

Note: The string after *MatchProduct* should be the trackpoint device name found in previous step.

```
Section "InputClass"
    Identifier  "Trackpoint Wheel Emulation"
    MatchProduct    "TPPS/2 IBM TrackPoint"
    MatchDevicePath "/dev/input/event*"
    Option      "EmulateWheel"      "true"
    Option      "EmulateWheelButton"    "2"
    Option      "Emulate3Buttons"   "false"
    Option      "XAxisMapping"      "6 7"
    Option      "YAxisMapping"      "4 5"
EndSection
```

### Restart your computer

Reference:
- <http://www.thinkwiki.org/wiki/How_to_configure_the_TrackPoint#xorg.conf.d>
- <http://forums.fedoraforum.org/showthread.php?t=280930>

