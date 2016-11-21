---
layout: post
title: Recover windows boot menu in grub2 after installing RHEL7
date: 2016-11-14 22:17:13 +0800
---

After installing RHEL7.3, Windows 10 disappeared from the grup boot menu.
It's easy to recover it by following steps.

- Enable EPEL repo

```shell
$ sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

- Install ntfs-3g

```shell
$ sudo yum install ntfs-3g
```

- Regenerate grub.cfg

```shell
$ sudo cp /boot/grub2/grub.cfg /boot/grub2/grub.cfg.old
$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```
