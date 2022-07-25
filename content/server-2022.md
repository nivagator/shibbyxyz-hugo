---
title: "state of the server 2022"
date: 2022-06-10T12:06:46-05:00
tags: ['hardware']
---

# state of the server 2022
June 10, 2022

## backstory 

When we moved back in 2015, I bought a Synology DS216j to use as a NAS for backups and a file server. Synology make a great little box that just worked for several years. The model I bought was on the budget end. It was slow to begin with and really struggled to even serve my Plex library. About this time, I was starting to explore self-hosting certain services and getting deeper into linux beyond the casual vps experimentation. I bought a Lenovo e31 tower off Craigslist with a Xeon E3-1245v2 and 16GB of ECC DDR3 to use as a server/homelab. 

Around this time, I was asked to do some contract work as a DBA/report developer for a friend and decided to use the e31 as a dev box. A friend of mine worked at VMWare and on that basis alone, I installed ESXi and ran a Windows server as a MS SQL and SSRS box along with a few linux server VMs for other tasks. It ran fine for a few years and I used it to learn quite a bit about server side linux without having to spin up a vps. It ran fine for a few years, but when the work stopped, I stopped maintaining it and the hardware sat idle. 

I've been watching Linus Tech Tips videos for a while. Over the years, but especially back in 2016, they have produced a bunch of content around Unraid systems. Some of them were the extreme content they are know for ([7 Gamers, 1 CPU - Ultimate Virtualized Gaming Build Log](https://youtube.com/watch?v=LXOaCkbt4lI)). But some of it was more "practical" if you can call it that: 

- [2 Gaming Rigs, 1 Tower - Virtualized Gaming Build Log](https://youtube.com/watch?v=LuJYMCbIbPk)
- [Use your Gaming PC's Extra Power as a NAS Ultimate Guide](https://youtube.com/watch?v=dpXhSrhmUXo)

The idea of running my primary desktop as a VM on a shared server machine was intriguing. I decided to wipe the e31 and start over. I originally installed TrueNAS as the host OS since it was free, but not knowing much about containerization or ZFS, I opted to give Unraid a go. It was exactly what I needed/wanted. The user interface is simple enough and makes apps, docker containers, shares, network distribution, even PCI passthrough a breeze. 

## today

I've replaced the Xeon with a consumer grade Ryzen cpu. Mostly for the ram upgrade. I looked into getting 32gb of DDR3, but the cost didn't make sense considering the additional benefits of a modern platform - things like PCIe slots instead of PCI. I've got 32tb of raw HDD disk space running through an LSI 9211, a bunch of docker services and even a gaming vm for my son running Pop_OS all on one box. I've added a Quadro P400 as a hardware transcoding device in Plex/Tdarr. I've added a  bluray drive to import movies. Plex has expanded to house my early 2000s music collection. I'm really enjoying the flexibility and stability of the Unraid platform. 

## plans

I made the switch to Linux as my primary OS in late 2019 and haven't looked back. In that time, I've found ways to solve problems using open source software and scripts. I have no real problem to solve that Unraid can't handle. Nevertheless, I can't help but tinker. I've kept the Xeon and recently purchased a Supermicro X9 series motherboard for it. I couldnt get the original lenovo board to recognize the LSI HBA I had and the additional PCIe slots and IPMI made it worth the move. I've also been heavy into the [Perfect Media Server](https://perfectmediaserver.com/) website and by extension [SelfHosted Show](https://selfhosted.show/) and [Linuxserver.io](https://www.linuxserver.io/). I want to replicate the functions I enjoy in the Unraid platform on a server I've built on my own really for no other reason than to learn. So far, I've messed with MergerFS, ZFS and Snapraid as well as expanded my docker-compose skills. I'm hoping to nuke and pave soon with Proxmox as the host OS, you know, for fun. 

The long term question is deployment. I don't plan to migrate my primary home server off of Unraid. But I do have the Synology that is largely unused, and the xeon system without a real purpose. I'm going to work out where and how to deploy these systems in a way that makes sense. Potentially, asking family members to run them in their homes as remote endpoints. 

This nerd energy is part of the driver to document more of these plans and learnings here. 