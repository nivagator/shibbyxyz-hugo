---
title: "Luks Crypt"
date: 2022-08-21T21:04:16-05:00
tags: ['linux']
draft: true
---

```bash 
sudo cryptsetup open /dev/sdd1 secret

mkdir /mnt/encrypted-secret

sudo mount /dev/mapper/secret /mnt/encrypted-secret

sudo umount /mnt/encrypted-secret

sudo cryptsetup close secret
```