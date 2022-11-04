# **Installation Documentation**

## **-Setup for Installation-**

#### ***1. Locate and select any mirror under United States from the link below***
- [ArchDownloads](https://archlinux.org/download/)

#### ***2. Download the ISO file***
- archlinux-2022.10.01-x86_64.iso

## **-Installation-**

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

root@archiso ~ # mkfs.fat -F32 /dev/sda1
```

#### ***10. Mount the root & EFI system partitions***
```shell
root@archiso ~ # mount /dev/sda2 /mnt

root@archiso ~ # mount --mkdir /dev/sda1 /mnt/boot
```

#### ***11. Download base package, Linux kernel, and firmware***
```shell
root@archiso ~ # pacstrap -K /mnt base linux linux-firmware
```

## **-Configuration-**

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

#### ***4. Add nano***
```shell
[root@archiso /]# pacman -Syu nano
```

#### ***5. Uncomment the following in /etc/locale.gen***
```shell
[root@archiso /]# nano /etc/locale.gen
```
- en_US.UTF-8 UTF-8
- Any other locales needed (None)

#### ***6. Generate locals***
```shell
[root@archiso /]# locale-gen
```
#### ***7. Create locale.conf & set LANG variable to "en_US.UTF-8"***
```shell
[root@archiso /]# nano /etc/locale.conf
```
#### ***8. Create hostname file & add preferred name (used: brayden)***
```shell
[root@archiso /]# nano /etc/hostname
```

#### ***9. Add the following to static table lookup for hostnames***
```shell
[root@archiso /]# nano /etc/hosts
```
- 127.0.0.1     localhost
- ::1           localhost
- 127.0.1.1     [name from hostname].localdomain    [name from hostname]

#### ***10. Update root password***
```shell
[root@archiso /]# passwd
```

#### ***11. Create user and user directory***
```shell
[root@archiso /]# useradd -G wheel,audio,video -m [name from hostname]
```

#### ***12. Create password for new user***
```shell
[root@archiso /]# passwd [name from hostname]
```

#### ***13. Download netctl and dependencies (use default when prompted)***
```shell
[root@archiso /]# pacman -Syu netctl

[root@archiso /]# pacman -Syu dhcpcd dhcp
```

#### ***14. Download Sudo package and uncomment to allow wheel group to execute any command***
```shell
[root@archiso /]# pacman -Syu sudo

[root@archiso /]# EDITOR=nano visudo
```

#### ***15. Download openssh***
```shell
[root@archiso /]# pacman -Syu openssh
```

## **-Boot Loader-**

#### ***1. Download grub and  efibootmanager***
```shell
[root@archiso /]# pacman -Syu grub efibootmgr
```

#### ***2. Install grub pt. 1***
```shell
[root@archiso /]# grub-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=Arch
```

#### ***3. Create grub config***
```shell
ERROR   [root@archiso /]# grub-mkconfig -o /boot/grub/grub.cfg
```

## **-Finish Network Configuration-**
#### ***1. Find network adaptor's name (found ens33)***
```shell
[root@archiso /]# ip link
```

#### ***2. Generate a network card profile & change the inteferface line to "Interface=[network adaptor's name here]" in it***
```shell
[root@archiso /]# cp /etc/netctl/examples/ethernet-static /etc/netctl/ens33

[root@archiso /]# nano /etc/netctl/ens33
```

#### ***3. Enable network card***
```shell
[root@archiso /]# systemctl enable dhcpcd

ERROR   [root@archiso /]# systemctl start dhcpcd
```

## **-Unmount-**
```shell
[root@archiso /]# umount -R /mnt
```

## **-Boot Time!-**

#### ***1. Reboot VM***
```shell
[root@archiso /]# reboot
```

#### ***2. Select "Arch" for boot***

# **Post Installation**