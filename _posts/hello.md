---
title: Hello World
date: 2022-08-08 11:13:00 -700
categories: [homelab,hardware]
tags: [servers,dell,supermicro]
---


# Welcome

Hello Yall.

ToDo:
* setup

```python
def ping(host):
    param = '-n' if platform.system().lower()=='windows' else '-c'
    command = ['ping', param, '1', host]
    try:
        response = subprocess.call(command, stdout=subprocess.DEVNULL, stderr=subprocess.STDOUT)
    except Exception:
        print('Something went wrong!')
        return False
    if response == 0:
        #print('{} is Online'.format(host))
        return True
    else:
        #print('{} is Offline'.format(host))
        return False
```