---
layout: post
title:  enable thinkpad t430s trackpoint scrolling on fedora 16
date:   2012-09-10 13:41:10
---

### create file `/etc/X11/xorg.conf.d/10-thinkpad-trackpad-scroll.conf` with following content.

    Section "InputClass"
        Identifier  "Trackpoint Wheel Emulation"
        MatchProduct    "TPPS/2 IBM TrackPoint|DualPoint Stick|Synaptics Inc. Composite TouchPad / TrackPoint|ThinkPad USB Keyboard with TrackPoint|USB Trackpoint pointing device
    |Composite TouchPad / TrackPoint"
        MatchDevicePath "/dev/input/event*"
        Option      "EmulateWheel"      "true"
        Option      "EmulateWheelButton"    "2"
        Option      "Emulate3Buttons"   "false"
        Option      "XAxisMapping"      "6 7"
        Option      "YAxisMapping"      "4 5"
    EndSection

### restart your computer

reference <http://www.thinkwiki.org/wiki/How_to_configure_the_TrackPoint#xorg.conf.d> & <http://forums.fedoraforum.org/showthread.php?t=280930>

