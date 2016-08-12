# Linux LVM Beginners Guide

# LVM - Beginners Guide

## Introduction

When I started playing with Linux, I always wondered how to scale storage quickly and easly, without mirroring or format an entire disk. 

LVM give us that possibility, imagine if you have a big pool of storage, and over that pool you are able to create small devices. When your device is full, you only need to take a "bit" of space from our big pool.

Mainly, there are three keywords that you need to memorize, **Physical Volume(pv)**, **Volume Group(vg)** and **Logical Volume(lv)**. An **PV** will be as it name says, an physical hard disk, so an **VG** is an group os **PV's**, that we'll split into **LV's**. 

#### Requirements:

  * Linux distro with LVM installed
  * Three HDD (sda/sdb/sdc)



## Setup our HDD for LVM

###### List all connected disks


    fdisk -l


At this time our brand new disk probably will say: `Disk /dev/dbb doesn’t contain valid partition table.`

###### This will put us on a special fdisk prompt


    fdisk /dev/sdb


###### Create new primary partition of LVM type


# Create a new partition
n
# Type is primary
p
# To set the start and end cylinder left default, press enter twice
# Change the type of partition
t
# Select the partition (if only exists one, the system will assume it)
1
# Hit L to preview the FS types, we must choose 8e (Linux LVM)
8e
# Preview all changes
p
# If everything is ok... Write it!
w


## Setup our LVM Pool
###### Create LVM Physical Volume (PV) on the partition we just created
    pvcreate /dev/sdb1

###### Now lets create our Volume Group (VG)
This will work like a pool of disks where we add new disks and create new LV's (Logical Volumes) from there.
    vgcreate vgpool /dev/sdb1

###### Create a Logical Volume (LV)
Finaly we will create a LV, witch will be a new mount point.
    # -L (Size)
    # -n (Name)
    lvcreate -L 3G -n lvweb vgpool
###### Format our new LV
    mkfs -t ext4 /dev/vgpool/lvweb
###### Create a mount point & mount it
    mkdir /web
    mount -t ext4 /dev/vgpool/lvweb /web

## Resizing Volumes
The best things about using is LVM, is that everytime that we need to recize, expand or reduce our system space, we can do it easly without having to move all our data to another disk.

**Exists three main options:**
  * resize - can shrink or expand PV's and LV's but not VG's
  * extend - can make VG's and LV's bigger but not smaller
  * reduce - can make VG's and LV's smaller but not bigger

## Expand a Volume Group
**In this case we'll expand our lvpool with our third disk (sdc) Add a new HD and format it like we did before for our lvweb.**

###### Create PV from our /dev/sdc1
    pvcreate /dev/sdc1

###### Now expand our lgpool by using expand
    vgextend vgpool /dev/sdc1

###### Now we are able to create an LV bigger
    lvcreate -L 1G -n lvdb vgpool

###### Like we did before, make a FS on it, and create a mount point
    mkfs -t ext4 /dev/vgpool/lvdb
    mkdir /db
    mount -t ext4 /dev/vgpool/lvdb /db

## Expand an LV (Logical Volume)
To expand our **lvweb** we'll use `lvextend`. In order to complete our task we'll use one flag that we used before `lvextend -L <space_allocated> <path_to_lv>`.

The flag `-L` accepts aritemetic operations. So...
    lvextend -L +200M /dev/vgpool/lvweb

The next and last step is expand the FS:
    resize2fs /dev/vgpool/lvweb

## Shrink an LV (Logical Volume)
**Caution!! This Operation can't be done whit the volume online.**
First unmount our **lvdb**:
    umount /db

Now you can reduce and make the FS:
    lvreduce -L -250M /dev/vgpool/lvdb
    mkfs -t ext4 /dev/vgpool/lvdb

Mount it: 
  `mount -a`
