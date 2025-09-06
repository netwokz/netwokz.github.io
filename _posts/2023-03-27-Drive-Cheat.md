---
title: Partition Cheat Sheet
date: 2023-03-27 22:38:00 -700
categories: [homelab,hardware]
tags: [servers,ubuntu,linux,plex]
---


# Common drive/volume resize

### Find drive
```bash
sudo fdisk -l
```

### Extend physical drive partition
```bash
# "3" being the partition number
sudo growpart /dev/sda 3
```

### Resize file system
```bash
sudo resize2fs /dev/sda3
```

# Common drive commands
```bash
df -h
du -sh

# if installed
sudo ncdu /
```
