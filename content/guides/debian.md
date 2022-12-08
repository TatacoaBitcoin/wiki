---
title: 'Debian Server'
metaTitle: 'Guías - Debian Server'
metaDescription: 'How-to guides'
---

https://njal.la/servers/add/
https://docs.microsoft.com/en-us/windows/wsl/install-win10

## LLave ssh

    $ ssh-keygen -t ed25519
    $ cat ~/.ssh/llave.pub
    $ ssh -i ~/.ssh/llave root@192.0.0.214
    $ apt upgrade
