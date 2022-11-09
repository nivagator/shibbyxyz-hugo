---
title: "Goodbye Synology"
date: 2022-10-28T17:40:27-05:00
draft: true
tags: ['projects']
---
My synology goes almost completely unused at this point. The plan is to migrate all data to the backup nebula server and take it offline.

Lets plan for this.

I have ~3.1TB of data on the synology. some of that is redundant backups from when i built the unraid server. Early on i was skeptical of the unraid platform and did not delete the date from the synology after migrating it to the new unraid box. Well, the unraid server has been up and online for well over a year at this point, and the backup copies do not need to be online all the time on the synology. 

the unraid server has ~8tb of free space spread across the installed drives. i could migrate all data to unraid, but i think i'll move it to the nebula server instead. honestly most of the data could be deleted since its a static backup. I have the space on nebula and that server doesnt do much to begin with.

Once the data is migrated, I will move the two 6tb disks to the unraid array replacing the very old 4tb drives

lets get started.

nebula is an old xeon box. supermicro board, lsi hba and 8 super old hard drives of various capacities. I have three 1tb drives configured in a zfs raidz1 pool and the other four drives in a mergerfs pool with the last drive serving snapraid support.

none of the data i want to move needs to be on the zfs pool other than maybe photos.

in order to transfer the files, i need some method to connect the synology shares to the proxmox file system. I can mount the synology shares directly to the proxmox server. I can either m

---

after a long time searching...........   
what i wanted was an lxc container with bind mounted host storage. i could not figure out how to create them in the gui, it wanted to create a new "volume" of a given size on the local storage and i didnt see an option to bind mount. lxc configs are stored on the host at `/etc/pve/lxc/1xx.conf`.  

[Linux Container Bind Mounts - Proxmox VE](https://pve.proxmox.com/wiki/Linux_Container#_bind_mount_points)  
For example, to make the directory /mnt/bindmounts/shared accessible in the container with ID 100 under the path /shared, add a configuration line such as:  
```mp0: /mnt/bindmounts/shared,mp=/shared```  
into /etc/pve/lxc/100.conf.  
Or alternatively use the pct tool:  
```pct set 100 -mp0 /mnt/bindmounts/shared,mp=/shared```  
to achieve the same result.  

now that is resolved... how do i share or mount synology... 

forget it. i pulled up [Perfect Media Server](https://perfectmediaserver.com/installation/manual-install-ubuntu/#nfs) and enabled nfs on the host. this might not be the "right" way to do it. but i really dont care at this point. 

im mounting both the proxmox storage and the synology storage to my unraid server to run the transfers. probably super inefficient using unraid as a passthrough. but im tired of arm wrestling with proxmox. not so fast. attempting to create a folder or write a file fails on unriad... glorious


- backups done
- files done
- photos
- homes
    - base done
    - cifs ggreer done
    - cifs mckennah done
    - cifs melanie done


mount -t cifs -o rw,vers=3.0,credentials=/mnt/remotes/.melaniecreds //192.168.50.30/home /mnt/remotes/synomelanie/  

rsync -r --info=progress2 /mnt/remotes/synomelanie/ /mnt/storage/synology/homes/melanie/

rsync -r --info=progress2 remotes/synologyfiles/ storage/synology/files/