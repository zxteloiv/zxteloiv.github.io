---
layout: page
title: virtual memory swap
math_support: mathjax
---



What is swappiness and how do I change it?

The swappiness parameter controls the tendency of the kernel to move processes out of physical memory and onto the swap disk. Because disks are much slower than RAM, this can lead to slower response times for system and applications if processes are too aggressively moved out of memory.

>  swappiness can have a value of between 0 and 100

>  swappiness=0 tells the kernel to avoid swapping processes out of physical memory for as long as possible

>  swappiness=100 tells the kernel to aggressively swap processes out of physical memory and move them to swap cache

The default setting in Ubuntu is swappiness=60. Reducing the default value of swappiness will probably improve overall performance for a typical Ubuntu desktop installation. A value of swappiness=10 is recommended, but feel free to experiment. Note: Ubuntu server installations have different performance requirements to desktop systems, and the default value of 60 is likely more suitable.

To check the swappiness value

`cat /proc/sys/vm/swappiness`

To change the swappiness value A temporary change (lost on reboot) with a swappiness value of 10 can be made with

`sudo sysctl vm.swappiness=10`

To make a change permanent, edit the configuration file with your favorite editor:

`sudo gedit /etc/sysctl.conf`

Search for vm.swappiness and change its value as desired. If vm.swappiness does not exist, add it to the end of the file like so:

`vm.swappiness=10`

Save the file and reboot.



