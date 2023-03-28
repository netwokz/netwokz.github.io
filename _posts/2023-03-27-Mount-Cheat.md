---
title: Drive Mount Cheat Sheet
date: 2023-03-27 19:43:00 -700
categories: [homelab,hardware]
tags: [servers,dell,supermicro]
---

# Network Share

```shell
sudo apt update
sudo apt install cifs-utils
sudo mkdir /mnt/win_share
```

### Setup credentials file usually in root directory

```shell
username=user
password=password
domain=domain
```

### May need to adjust permissions
```shell
sudo chmod 600 /etc/win-credentials
```
### Mount share
```shell
sudo mount -t cifs -o credentials=/etc/win-credentials //WIN_SHARE_IP/<share_name> /mnt/win_share
```
### Auto mount at boot
```shell
sudo nano /etc/fstab
# <file system>             <dir>          <type> <options>                                                   <dump>  <pass>
//WIN_SHARE_IP/share_name  /mnt/win_share  cifs  credentials=/etc/win-credentials,file_mode=0755,dir_mode=0755 0       0

# Mount all fstab mounts
mount -a
```
