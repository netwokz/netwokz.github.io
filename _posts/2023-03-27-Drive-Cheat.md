---
title: Partition Cheat Sheet
date: 2023-03-27 22:38:00 -700
categories: [homelab,hardware]
tags: [servers,ubuntu,linux,plex]
---


# Common drive/volume resize


### Find drive
```shell
sudo fdisk -l
```

### Extend physical drive partition
```shell
# "3" being the partition number
growpart /dev/sda 3
```

### Update Proxmox drive
```shell
pvdisplay
# Instruct LVM that disk size has changed
sudo pvresize /dev/sda3
# Check new PV
pvdisplay
```

### Extend LV
```shell
# View starting LV
$ lvdisplay
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv
  LV Name                ubuntu-lv
  VG Name                ubuntu-vg
  LV UUID                KfMCNz-e2DU-eUXJ-vXA8-Uk0q-DDTW-vnfE27
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2021-06-13 15:24:01 +0000
  LV Status              available
  # open                 1
  LV Size                <7.00 GiB
  Current LE             1791
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
# Resize LV
$ sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
Size of logical volume ubuntu-vg/ubuntu-lv changed from <7.00 GiB (1791 extents) to 1.46 TiB (383743 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.
# View changed LV
$ lvdisplay
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv
  LV Name                ubuntu-lv
  VG Name                ubuntu-vg
  LV UUID                KfMCNz-e2DU-eUXJ-vXA8-Uk0q-DDTW-vnfE27
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2021-06-13 15:24:01 +0000
  LV Status              available
  # open                 1
  LV Size                1.46 TiB
  Current LE             383743
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
  ```
  
  ### Resize file system
  ```shell
  resize2fs /dev/ubuntu-vg/ubuntu-lv
  ```
