# **Installation Documentation**

## **Setup for Installation**

#### ***1. Locate and select any mirror under United States from the link below***
- [ArchDownloads](https://archlinux.org/download/)

#### ***2. Download the ISO file***
- archlinux-2022.10.01-x86_64.iso

## **Installation**

#### ***1. Select "Create a New Virtual Machine in VMware Workstation 16 Pro"***

#### ***2. Select "Custom (advanced)"***

#### ***3. Select "Workstation 16.2.x"***

#### ***4. Select"Installer disc image file (iso):"*** 
    Locate the ISO file previously downloaded (archlinux-2022.10.01-x86_64.iso)

#### ***5. Select"Linux" and "Linux 5.x kernal 64-bit" for "Guest Operation System" and "Version" respectively***

#### ***6. Rename to Arch Linux if desired***

#### ***7."Processors" Settings***
-  Number of processors: 1
- Number of cores per processor: 2

#### ***8. Set "Memory for this virtual machine:" to 2048MB (2GB)***

#### ***9. Select "Use network address translation (NAT)"***

#### ***10. Select "LSI Logic (Recommended)"***

#### ***11. Select "SCSI (Recommended)"***

#### ***12. Select "Create a new virtual disk"***

#### ***13. Set "Maximum disk size (GB):" to 15.0 & Select "Store virtual disk as a single file"***

#### ***14. Change to "ArchLinux.vmdk" if desired***

#### ***15. Hit Finish***

#### ***16. Right click on the VM and select "Open VM directory"***

#### ***17. Open "[VM name].vmx" with a text editor***

#### ***18. Insert "firmware="efi"" in/on line 2***

## **Congrats, you've done it!**

# **Setup Documentation**

#### ***1. Run the VM***

#### ***2. Press enter on "Arch Linux install medium (x86_64, UEFI)"***

#### ***3. Verify the boot mode. If it fails, then you might be booted in BIOS***
```shell
root@archiso ~ # ls /sys/firmware/efi/efivars
```

#### ***4. Check internet connection***
```shell
root@archiso ~ # ip link

root@archiso ~ # ping google.com
```
*(hint: ctrl+c to stop pinging)*

#### ***5. Check system clock***
```shell
root@archiso ~ # timedatectl status
```

#### ***6. Find block device***
```shell
root@archiso ~ # fdisk -l
```

#### ***7. Use fdisk to edit***
```shell
root@archiso ~ # fdisk /dev/sda
```

#### ***8. Create two new partitions***
1. sda1
```shell
Command (m for help): n
...

Select (default p): p

Partition number (1-4, default 1): 1

First sector (2048-31457279, default 2048): 2048

Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-31457279, default 31457279): +500M
```
2. sda2
```shell
Command (m for help): n
...

Select (default p): p

Partition number (2-4, default 1): 2

First sector (1026048-31457279, default 1026048): 1026048

Last sector, +/-sectors or +/-size{K,M,G,T,P} (1026048-31457279, default 31457279): 31457279
```
Check partitions
```shell
Command (m for help): p
```
Save and exit
```shell
Command (m for help): w
```

#### ***9. Format partitions***
```shell
root@archiso ~ # mkfs.ext4 /dev/sda2

root@archiso ~ # mkfs.fat -F 32 /dev/sda1
```

#### ***10. Mount the root & EFI system partitions***
```shell
root@archiso ~ # mount /dev/sda2 /mnt

root@archiso ~ # mount --mkdir /dev/sda1 /mnt/boot
```

#### ***11. Install base package, Linux kernel, and firmware***
```shell
root@archiso ~ # pacstrap -K /mnt base linux linux-firmware
```

## **Configuration**

#### ***1. Create fstab file***
```shell
root@archiso ~ # genfstab -U /mnt >> /mnt/etc/fstab
```

#### ***2. Change root into the new system***
```shell
root@archiso ~ # arch-chroot /mnt
```

#### ***3. Change time zone***
```shell
[root@archiso /]# ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime

[root@archiso /]# hwclock --systohc
```
(Second command generates /etc/adjtime)

#### ***4. Uncomment the following in /etc/locale.gen (Tip: Must use exit before using nano)***
```shell
root@archiso ~ # nano /mnt/etc/Locale.gen
```
- en_US.UTF-8 UTF-8
- Any other locales needed (None)

#### ***5. Generate locals (chroot again)***
```shell
[root@archiso /]# locale-gen
```
#### ***6. Create locale.conf & set LANG variable to "en_US.UTF-8"***
```shell
[root@archiso /]# touch /etc/locale.conf

root@archiso ~ # nano /mnt/etc/locale.conf
```
#### ***7. Create hostname file***
```shell
[root@archiso /]# touch /etc/hostname

root@archiso ~ # nano /mnt/etc/hostname
```
