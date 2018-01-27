## Setup Arch Linux with UEFI, LVM, LUKS and hardened system
A secure Arch linux setup with UEFI, encrypted LVM LUKS and hardened system. Download the current version of Arch Linux which can be downloaded at archlinux.org/download. If Arch is being setup on a virtual machine make sure to change settings to UEFI.

### Setup the partitions

#### 1. Check network connectivity and setup mirrors
UEFI is required for this tutorial. Check if your sysem is running UEFI by entering the following:
>ls /sys/firmware/efi
(show example)

Setup wifi or ethernet so the packages can be downloaded from the mirrors later on.
>wifi-menu
(show picture)

Check the connectivity by pinging Google.
>ping -c 3 8.8.8.8

Get Mirrorlists from your location and add them to configuration file.
https://www.archlinux.org/mirrorlist/
>vim /etc/pacman.d/mirrorlist

#### 2. Setup partitions (EFI and LVM)
Gdisk can be used to identity the partitions currently on your system and create new partitions
>gdisk /dev/sda
>gdisk -o 
>gdisk -n
>gdisk -w


#### 3. Encrypt the partitions (LUKS)
To encrypt the our entire system we will be using LUKS. The name you choose for your LVM is the last step will be used for the "lvm" here. This will encrypt the LVM /dev/sda2 with LUKS.
>cryptsetup luksFormat /dev/sda2
>cryptsetup open —type luks /dev/sda2 lvm

#### 4. LVM setup
Setup phsical volume
>pvcreate /dev/mapper/lvm

Setup volume and volume name
>vgcreate volume /dev/mapper/lvm 

Logical volume setup. The swap lvcreate is optional depending on if you need / want swap space. The swap space does not require a large amount of space, 4GB is used. The root size will depend on how big your disk space is, in my example i am going with 20G. The last option is home which will allocate any other space avaliable to home. 

>lvcreate -L4G volume -n swap
>lvcreate -L20G volume -n root
>lvcreate -l FREE100% volume -n home

#### 5. Mount and format partitions   
Format the partitions with ex54 and swap if used in previous steps.

>mount /dev/mapper/volume-root /mnt
>mkdir /mnt/home
>mkdir /mnt/boot
>mount /dev/mapper/volume-home /mnt/home
>mount /dev/sda1 /mnt/boot
>swapon /dev/mapper/volume-swap

### Setup base system


#### 6. Install base system

#### 7. Generate fstab

#### 8. Chroot

