---
title: "Rolling"
date: 2022-08-29T15:26:09-05:00
tags: ['linux']
---

I've been running an arch-based, rolling release linux distribution for the past year. I've heard stories of the challenges with a bleeding-edge os, but never really had any major issues. I have had some software issues: i remember an ffmpeg update broke handbrake for a few days, there were issues at times with the greenwithenvy app i use for my nvidia card. Nothing ever "broke" my install until last week. 

A recent grub update (2.06.r322) caused issues booting my Endeavour OS install on my laptop and desktop. [They have a good news post on this](https://endeavouros.com/news/full-transparency-on-the-grub-issue/) that explains the details. Grub added a new call that was incompatible with other versions. They also [published steps](https://forum.endeavouros.com/t/the-latest-grub-package-update-needs-some-manual-intervention/30689) to reinstall grub and correct the issue. The news post mentions that Arch is publishing a version without the offending grub commit and working upstream to correct the problem going forward. 

It took very little effort to correct. Boot to live image, mount the partitions and arch-chroot in to the system. Running a simple `grub-install` corrected the problem and allowed me to boot normally. 

I was able to access my system while the issue persisted. I have several OS installs on each system. Booting into another grub instance and selecting choose the `initramfs fallback` for my i3 endeavour system would boot successfully. I had never had a reason to run the fallback initramfs before.

 The challenge in getting this resolved was figuring out what exactly to run web searches for. I initially searched for arch initramfs fallback, but it was too generic for the issue and led to several `mkinitcpio` solutions. There were several steps in between but i was finally able to find the solution when i targeted endeavour OS specifically in my search query. 

 Fun times in linux land.