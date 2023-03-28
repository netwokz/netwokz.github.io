---
title: Hello World
date: 2023-03-27 19:43:00 -700
categories: [homelab,hardware]
tags: [servers,dell,supermicro]
---

# Network Share

```bash
sudo apt update
sudo apt install cifs-utils
sudo mkdir /mnt/win_share
```

## Setup credentials file
### Usually in root directory

```bash
username=user
password=password
domain=domain
```

### May need to adjust permissions
```bash
sudo chmod 600 /etc/win-credentials
```
### Mount share
```bash
sudo mount -t cifs -o credentials=/etc/win-credentials //WIN_SHARE_IP/<share_name> /mnt/win_share
```
