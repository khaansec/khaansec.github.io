---
layout: post
title: Resizing Ubuntu VM Disks in ProxMox
subtitle: Taking notes and sharing knowledge
cover-img: /assets/img/proxmox.png
thumbnail-img: /assets/img/proxmox.png
share-img: /assets/img/proxmox.png
tags: []
---

Quick caveat, this is a quick post on some notes to remind me of what works the most reliably for my setup. I encourage you to check out the referenced article before actually following any of this advice here.

It seems like every time i run out of space on one of my 32 GB default size VMs, i repeat the same research to try and remember the quickest way and what commands to run to achieve a resize.

First things first, head over to the hardware section of the VM you want to resize, and select `resize` from the disk action drop down after clicking on the disk you want to resize.

Now you should see a small pop up to specify by how many GB you would like to increase the size. 

Once the resize task is complete, we need to go into the VM itself, in this case Ubuntu, and tell the VM how to find the rest of the disk
`sudo growpart /dev/sda 3`
`sudo  lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`

Depending on your setup, there are likely much easier ways to do this. I just like to get experience in Linux administration when i can.


credit to: https://packetpushers.net/blog/ubuntu-extend-your-default-lvm-space/