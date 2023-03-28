---
title: Hello World
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

## Setup credentials file
### Usually in root directory

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
