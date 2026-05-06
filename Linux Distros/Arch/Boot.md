# Boot
Fix **Arch** boot problem using **LVM**.  
System crushed while updating now it could not boot anymore such that the boot partition got corrupted, but the the main system was okay.  
When booted it says missing firmare.  

## Restore using Chroot
Use bootble image.  
Mount **LVM** partition.  
```sh
modprobe dm-mod
vgchange -ay
mount -o subvol=@ /dev/ArchinstallVg/root /mnt
mount /dev/sda1 /mnt/noot
arch-chroot /mnt
```

### Reinstall Firmware from cache
```sh
pacman -U /var/cache/pacman/pkg/linux-17.7.pkg.tar.zst
```


## Create boot partition
Boot partition requires **fat32** as **ESP** with atleast 1GB space.  
```sh

```



```sh
mount -t proc /proc /mnt/proc
mount /dev /mnt/dev
mount /sys /mnt/sys
mount /run /mnt/run
```

Chroot and install required packages.  
```sh
chroot /mnt /bin/bash
```

```sh
pacman -Syu
```

Install linux, linux-headers.
```sh
pacman -S linux linux-headers
```  

Add **boot** directory on **/mnt** directory,  
```sh
mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
```

Add boot entries.
```ini
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=/dev/mapper/ArchinstallVg-root rw rootflags=subvol@ lvmwait=ArchinstallVg-root
```


Use **lsblk** and copy **UUID** for the boot and paste in ***/etc/fstab***.  