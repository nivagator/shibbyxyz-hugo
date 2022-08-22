---
title: "ZFS and mergerfs"
date: 2022-08-08T22:28:15-05:00
draft: true
---

working on nebula.  
building a mergerfs pool with snapraid parity and a zfs pool  

zfs pool  
`sudo zpool create tank raidz1 -m /mnt/tank -o ashift=12 /dev/disk/by-id/ata-WDC_WD10EADS-00L5B1_WD-WCAU4C256190 /dev/disk/by-id/ata-WDC_WD10EACS-00D6B1_WD-WCAU48024630 /dev/disk/by-id/ata-WDC_WD10JPVX-22JC3T0_WD-WX71A1693Z8H`  

```bash
gavin@nebula:~$ zpool status
  pool: tank
 state: ONLINE
  scan: scrub repaired 0B in 0 days 00:00:01 with 0 errors on Wed May 11 11:17:26 2022
config:

	NAME                                          STATE     READ WRITE CKSUM
	tank                                          ONLINE       0     0     0
	  raidz1-0                                    ONLINE       0     0     0
	    ata-WDC_WD10EADS-00L5B1_WD-WCAU4C256190   ONLINE       0     0     0
	    ata-WDC_WD10EACS-00D6B1_WD-WCAU48024630   ONLINE       0     0     0
	    ata-WDC_WD10JPVX-22JC3T0_WD-WX71A1693Z8H  ONLINE       0     0     0

errors: No known data errors
```

had trouble directing traffic when combining the snapraid pool with zfs pool under the same mergerfs mount. it has to do with the policy in mergerfs. more reading is needed

i want to deploy photoprism. i plan to do this on nebula. i want to rsync my nextcloud photos to nebula and run photoprism



nebula hdd spindown rate `/etc/hdparms`  
need to figure out how to control when and how the hdds spin down.


nebula: zfs or mergerfs destination
! reminder ! i need to change the mergerfs policy and put zfs back into the mergerfs fuse


nebula - changed the mergerfs policy to epmfs - existing path most free space. this should in theory keep the data i want on zfs separate from the general snapraid pool but still all accessible from /mnt/storage

i need to determine snapraid and zfs mainenance items in addition to backup solutions. 


proxmox installed without issue. it automatically detected and imported my zfs pool and datasets. installed snapraid and mergerfs on the root. replicated the fstab to include the snapraid and tank/fuse. 