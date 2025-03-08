---
title: Misc Cheat Sheet
date: 2023-04-18 08:30:00 -700
categories: [homelab,hardware]
tags: [servers,ubuntu,linux,plex,cheat]
---

### Git Credentials Management
## Windows:
```bash
git config --global credential.helper wincred
```
## Linux:
```bash
git config --global credential.helper store
```

### Windows ssh-copy-id
```bash
type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"
```

### Run script from URL
```bash
bash <(curl -s http://mywebsite.example/myscript.sh)
```


### Fix VM duplicate IP

```bash
# Must be ran as root!
sudo su
echo -n >/etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
```

### Fix VM "disk not bootable"
```bash
# Write new table
gdisk /dev/zvol/vm-disk
```
