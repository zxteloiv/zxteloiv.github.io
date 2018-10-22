---
layout: page
title: RAID configuration
math_support: mathjax
---


https://www.digitalocean.com/community/tutorials/how-to-create-raid-arrays-with-mdadm-on-ubuntu-16-04#creating-a-raid-1-array

https://raid.wiki.kernel.org/index.php/A_guide_to_mdadm

https://raid.wiki.kernel.org/index.php/RAID_setup

### clear the existing array

```
cat /proc/mdstat
sudo umount /dev/md0
sudo mdadm --stop /dev/md0
sudo mdadm --remove /dev/md0
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
sudo mdadm --zero-superblock /dev/sdc
sudo mdadm --zero-superblock /dev/sdd
sudo nano /etc/fstab
# /dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0
sudo nano /etc/mdadm/mdadm.conf
# ARRAY /dev/md0 metadata=1.2 name=mdadmwrite:0 UUID=7261fb9c:976d0d97:30bc63ce:85e76e91
sudo update-initramfs -u
```

### create RAID1

```
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
cat /proc/mdstat
sudo mkfs.ext4 -F /dev/md0
sudo mkdir -p /mnt/md0
sudo mount /dev/md0 /mnt/md0
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
sudo update-initramfs -u
echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
```


