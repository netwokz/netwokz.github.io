---
title: Arch Cheat Sheet
date: 2023-04-28 21:25:00 -700
categories: [homelab, hardware]
tags: [servers, arch, linux, cheat]
---

### Arch install WiFi connection

```bash
$ iwctl
[iwd]# station wlan0 connect SSID
```

### Arch install fix keyring

```bash
$ killall gpg-agent
$ rm -rf /etc/pacman.d/gnupg/*
$ pacman-key --init
$ pacman-key --populate archlinux
$ pacman -Sy
```
