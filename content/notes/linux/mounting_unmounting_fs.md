---
title: Mounting and Unmounting file systems
date: 2022-10-20T02:47:22+03:00
draft: false
author: 'Evans Owamoyo'
categories: [Notes]
tags: [linux, fs, rhel]
comments: false
---
# Mounting and Unmounting File Systems

## Mounting File Systems Manually
mount <file_system> <location>

* There are two ways to specify file_system on a disk partition
- name of device in /dev
- uuid 

**Command `lsblk` list the details of block devices available**

`/mnt` is the a temporary mount point 

## Mounting by UUID
* use `lsblk -fp` to get the full path and uuid of devices
```bash
mount UUID="3242424-4242-4242424" /mnt/data
```
* use `fdisk -l` to view disks currently on your machine

## Automatic mounting of Removable Storage Devices
if on GUI, removable device is mounted at `/run/media/<USER>/<LABEL>`. 

## Unmounting File Systems
* shutdown unmounts all file systems automatically
to unmount a file system
```bash
umount /mnt/data # if pwd is /mnt/data, it will error because shell is still using the directory as current wd.
```

* lsof list all open files and processes accessing them. Useful for finding which process is preventing umount
```bash
lsof /mnt/data
```

```bash
parted /dev/<id> print 
parted /dev/<id> mklabel gpt # make a gpt partition table
i) parted /dev/<id>
ii) mkpart # to make a new partition

#alternative one-liner
parted /dev/<id> mkpart <name> xfs 2048s 1000MB # parted device mkpart name fs_type start_sector end_sector

udevadm settle # to wait for system to detect the new partition and to create the ass. device file under the /dev
```

## Deleting Partitions
```bash
parted
print
rm <number>
```

## Creating File Systems
there are two common file systems in rhel "xfs" and "ext4"
mkfs.xfs /dev/sdb

# Mounting File Systems
we have looked at the manual way of mounting file system
```bash
mount <device> <location>
```

# Managing Swap Space
* an area of a disk under the control of the Linux kernel memory management subsystem. The kernel uses swap space to supp. the system RAM by holding inactive pages of memory. The combined system RAM plus swap space is called *virtual memory*.
* RAM and swap space recommendations


|RAM | Swap Space | Swap Space if allowing for Hibernation |
|:----:|:----:|-----:|
|2GiB or less | 2x ram | 3x ram |
|2 < x < 8 | = ram | 2x ram |
|8 < x < 64 | >= 4gb | 1.5x ram |
|\>64 | >= 4gb | Hiberation is not recommended |

## Creating a swap space
* create a partition with a fs type of linux-swap
```bash
parted /dev/sdb
print
mkpart
> swap1 # name
> linux-swap # fs type
> 1001MB # start
> 1257MB # end

$ udevadm settle # to wait for the partition to be created
```

* creating a swap signature to the device
```bash
mkswap /dev/sdb2 # to setup swapspace
```

## Activating a Swap space
```bash
free OR swapon --show # to inspect the available swap spaces.
swapon /dev/sdb2 # swapoff deactivates a swapspace 
free
```

* the `/etc/fstab` file is where configurations about disks and swap spaces are loaded from start time of the machine.  
`UUID=<uuid> swap swap defaults 0 0`

## Setting the swap space priority
by default the system uses swap spaces in series. However, swap spaces until it is full, then it uses the second swap space. However, you can define a priority for each swap space to force that order. the default is `-2`.  
```
UUID=adsfdsfdsafdsafsdf swap swap pri=4  0 0  
UUID=asdfsdafsdafsdfsda swap swap pri=10 0 0  
```
* the highest priority is used.


