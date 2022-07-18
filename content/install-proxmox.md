---
title: "install proxmox"
date: 2022-06-23T12:06:46-05:00
---
# install proxmox
June 23, 2022

My primary server is an Unraid box running as my NAS and several services in Docker. I still have old server hardware. You can see the current hardware stack [here](setup.html).

In the last couple of months, I reconstructed the old hardware and attached a bunch of very old hard drives to use as more of a home lab and called it `nebula`. The goal was to learn/experiment with some of the technologies I have been exposed to through [Perfect Media Server](https://perfectmediaserver.com/) like ZFS, mergerfs, snapraid and rolling my own docker compose. I reflexively installed ubuntu as the base os, created a RAIDZ pool and an array of disks in snapraid with no issues. 

Never able to leave good enough alone, I decided I wanted to nuke and pave with a proxmox install. I've got a lot of experience with ubuntu specific server installs and wanted to do something different and have the option of running services in vms. This would also give me experience with LXC containers. My goal in this install swap was to keep the snapraid array and ZFS pools consistent and just migrate them to the new os.

I started by copying the data on the host ssd to my local tower. I used rsync to copy the docker data and configs. The first attempt there were permission errors. A quick google reminded me you can run rsync as sudo. No the most secure option but it worked just fine for this one time transfer. The next issue was the photoprism docker container's cache was 12GB. I know this can be recreated and didn't care to spend the time transferring all 12GB. I added the cache as an exclude. I landed on this command: 

```bash
rsync -av -e ssh --rsync-path="sudo rsync" --exclude='photoprism/storage/cache' nebula:/home/gavin/docker/ docker/
```

Once that was done, I copied the `.ssh` dir, the `fstab`, the `/etc/snapraid.conf` config file and exported the ZFS pool with `zpool export tank`. With that done, I had everything needed to recreate the data storage array on the new host os. 

Another minor objective of mine has been to manage this home lab entirely headlessly. I swapped the original lenovo motherboard with a supermicro X9 with IPMI support to give me remote management (and PCIe) options. It was fairly simple to log into the IMPI interface from my desktop machine, load the KVM, attach an iso from my unraid nfs share and power on the machine without requiring physical access to the hardware. 

The install was very easy. I didn't do much more than accept the defaults and choose the correct networking device. To my surprise, the exported ZFS pool was automatically detected, imported and mounted without any interaction from me. Getting snapraid and mergerfs installed were simple `apt install` commands from the command line. I then created the requisite mount points and updated the proxmox fstab and a simple `mount -a` had the snapraid devices mounted. `snapraid sync` was successful and it was back in business. One more edit to the fstab to add the mergerfs fuse and the data array was completely restored. 

No other services were added, but I have a functioning proxmox box. I do need to research proxmox user best practices, LXC vs Docker and restic enpoint integration (replicating what i had on the ubuntu system). Should be fun. Eventually this box could go live at another physical location as a remote backup endpoint, but thats for another day.