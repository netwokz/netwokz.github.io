---
title: Misc Cheat Sheet
date: 2023-04-18 08:30:00 -700
categories: [homelab,hardware]
tags: [servers,ubuntu,linux,plex,cheat]
---


### Windows ssh-copy-id
```bash
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```

### Fix VM duplicate IP

```bash
# Must be ran as root!
sudo su
echo -n >/etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
```
