---
title: "LUKS Crypt"
date: 2022-08-21T21:04:16-05:00
tags: ['linux']
---

This is handy to encrypt a portable flash drive. Its also relevant for any linux install on with encryption. My Pop_OS! install was encrypted and I still have and access the boot disk. These instructions are the basics of LUKS setup and usage. 

# creating an encrypted partition

### attach media and check its label:
```bash
sudo dmesg | tail 
sdd: 
sdd1: 14.3 GiB

sudo fdisk -l /dev/sdd1
Disk /dev/sdd1: 14.32 GiB, 15374221312 bytes, 30027776 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

### write it with random data to prepare for encryption:

```bash
$ sudo dd if=/dev/urandom of=/dev/sdd1 bs=4M status=progress
```

### erase and create a new partition table and partition
```bash
sudo fdisk /dev/sdd1

Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.

Command (m for help): o
Created a new DOS disklabel with disk identifier 0xd756b789.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-31116287, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-31116287, default 31116287):

Created a new partition 1 of type 'Linux'.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

if a specific partition size is desired, the Last sector command can be supplied an incremental value such as `+25M` or `+2G` instead of accepting the default.

### use LUKS to encrypt the new partition
```bash
sudo cryptsetup luksFormat /dev/sdd1

WARNING!
========
This will overwrite data on /dev/mmcblk0p1 irrevocably.

Are you sure? (Type uppercase yes): YES
Enter passphrase for /dev/mmcblk0p1:
Verify passphrase:
```

### open the encrypted partition: 
```bash
sudo cryptsetup open /dev/sdd1 secret
Enter passphrase for /dev/sdd1:
```

### create a filesystem
```bash
sudo mkfs.ext2 /dev/mapper/secret -L filesystem-label
Creating filesystem with 9216 1k blocks and 2304 inodes
Superblock backups stored on blocks:
        8193

Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done
``` 

### close the partition 
```bash
sudo cryptsetup close secret
```

# using the encrypted device

```bash 
# open the encrypted partition
sudo cryptsetup open /dev/sdd1 secret

# create mount dir
mkdir /mnt/encrypted-secret

# mount the open partition
sudo mount /dev/mapper/secret /mnt/encrypted-secret

# the device can now be accessed and used 

# unmount the open partition
sudo umount /mnt/encrypted-secret

# close the encrypted partiion
sudo cryptsetup close secret
```