# Storage
## Introduction  
Tools used:  
- **dd** - Low-level disk copying tool.
- **lsblk** - List block devices. View all attached storage, partirions, mount points, sizes. 
- **fdisk** - Partition disk(MBR-based). Create, delete and modify partitions using the **MBR(DOS)** partition table. Manage partitions on legacy or small (<2TB) disks.
- **gdisk** - Partition disk(GPT). Partition large(>2TB) disks or UEFI systems.
Use parted if you want a tool that supports both MBR and GPT interactively.


## Mount
**Block devices** - found under `/dev/` directory. Represent piece of hardware that transfers data in blocks, typically disks (HDDs, SSDs, USB drives).  
Data is written or read in block state.  
To see block devices, use `lsblk`.  
```sh
lsblk
```

or `ls` on `/dev/`.  
```sh
ls -l /dev/ | grep "^b"
```  
- `grep "^b"` - Filters for block devices only (lines starting with b).  

Major/Minor numbers:  
- 1 - RAM  
- 3 - HARD DISK or CD ROM  
- 6 - PARALLEL PRINTERS  
- 8 - SCSI DISK  

### Disk partitions  
List disks and partitions.
```sh
lsblk
```

In this case USB drive is **/dev/sdb**.  
```sh
gdisk -l /dev/sdb
```  


Create new partition.
```sh
gidisk /dev/sdb
```

type `n` to create new partition.
type `w` to write new partition.


### File systems
After creating partition, we must create file system. Mount file system to a directory.
Extended filesystem available filesystem in linux:
- ext2
	- maximum file size 2TB.
	- volume size 4TB.
	- support compression.
	- supports linux permissions.
	- log crash recovery.

- ext3
	- maximum file size 2TB.
	- volume size 4TB.
	- uses journal.
	- Backward compatiple.

- ext4
	- file size 16TB.
	- 1 exabyte volume size.
	- use chksum for journal.
	- backwards compatible.

## Volume  
Create volume.
```sh
mkfs.ext4 /dev/sdb1
```

Create dir to mount.
```sh
mkdir /mnt/myusb
```

Mount.
```sh
mount /dev/sdb1 /mnt/myusb
```

Verify.
```sh
mount | grep /dev/sdb1
```

Make this drive to mount after reboot, edit `fstab`.
```sh
vim /etc/fstab
```

```conf
/dev/sda1    /    ext4     defaults,relatime,error=panic    0    1 ~
```

**Fields**
Filesystem - such as /dev/sda to be mounted.
Mountpoint - directory to be mounted on.
Type - Example ext2, ext3, ext4.
Options - Such as RW = Read-write, RO = Read Only.
Dump - 0 = ignore, 1 = take backup.
Pass - 0 = ignore, 1 or 2 = FSCK filesystem check enforced.



### Types of storage
- **DAS** - Direct Attached Storage.
	- Block storage.
	- Fast & reliable.
	- Dedicated to single host.
	- Affordable.
	- Ideal for small biz.

- **NAS** - Network Attached Storage.
	- NFS/CIFS.
	- Reasonably fast & reliable.
	- File based storage.
	- Shared storage.
	- Not suitable for OS install.

- **SAN** - Storage Area Network.
	- FC(fibre channel protocol) or iSCSI.
	- Block storage.
	- Fast, secure & reliable.
	- High availability.
	- Expensive.

#### NFS
 Maintains config file at `/etc/exports`.
 example:
```conf
/software/repos 10.0.3.1 10.0.3.2 10.0.3.5
```

`exportfs -a` exports all the mounts in the `/etc/exports` to the network.
```sh
exportfs -o 10.0.3.5:/software/repos
```

this exports directory manually. then mount the directory on the client.
```sh
mount 10.0.3.1:/software/repos /mnt/software/repos
```



### Logical Volume Manager(LVM)
**Logical Volume Manager** - enables grouping multiple physical volume into logical volume.
Allows the disks to be resized dynamically.

#### Physical Volume  
Use `pvcreate` to initialise a physical volume(PV) for use with lvm. Prepares storage device like a disk or partition to be used as part of a volume group(VG), which can then be dived into logical volumes(LVs).
```sh
pvcreate /dev/vdc
```

Verify.
```sh
pvdisplay or pvs
```

#### Volume Group  
```sh
vgcreate iko_vg /dev/vdc
```

Verify.
```sh
vgdisplay
```

Extend.
```sh
vgextend iko_vg /dev/vdb
```

Verify.
```sh
pvdisplay or pvs
```

#### Logical Volume  
```sh
lvcreate -L 5G -n vol1 iko_vg
```

Verify.
```sh
lvdisplay or lvs
```

Create filesystem.
```sh
mkfs.ext4 /dev/iko_vg/vol1
```


#### Resize  
Check available space.
```sh
vgs
```

Resize volume.
```sh
lvresize -L +1G -n /dev/iko_vg/vol1
```

Resize filesystem.
```sh
resize2fs /dev/iko_vg/vol1
```


#### Delete
Check mounted volume.
```sh
df -h
```

Unmount any logical volume if applicable.
```sh
umount /mnt/myusb
```

Remove logical volume.
```sh
lvremove /dev/iko_vg/vol1
```

Deactivate the volume group.
```sh
vgchange -an iko_vg
```

Delete the volume group.
```sh
vgremove iko_vg
```

Remove physical volume.
```sh
pvremove /dev/vdc 
```

## Storage capacity  
`du` command is used to confirm storage space is accessible.  
```sh
du -h
```
- `-h`: show in human readable format.  


## dd
Best for cleaning disks before formatting and low level copying.  
### Create bootable linux ISO
Use **dd** tool to create bootable **linux.iso**.
```sh
sudo dd if=/home/dere/Desktop/kali-linux-2024.2-live-amd64.iso of=/dev/sdb bs=4M status=progress oflag=sync
```
- `dd`: Low-level disk copying tool.  
- `if=/dev/zero`: Input file is a stream of zeroes.  
- `of=/dev/sdb`: Output file is the entire disk /dev/sdb.  
- `bs=1M`: Block size is 1 Megabyte (faster than default 512 bytes).  
- `status=progress`: Shows real-time progress.  

### Clean disk  
#### HDD  
```sh
sudo dd if=/dev/zero of=/dev/sdb bs=1M status=progress
```  
- Writes: All zeroes (0x00).  
- Speed: Fast (zeroes are easy to generate and write).  
- Best for: Quick wipe before reuse, reinstallation, or repartitioning.  

```sh
sudo dd if=/dev/urandom of=/dev/sdb bs=1M status=progress
```  
Writes: Cryptographically random data.  
Speed: Slow (generating randomness is CPU-intensive).  
Security: More secure — harder to recover data.  
Best for: Erasing sensitive data on HDDs (not SSDs), or preparing a drive for encryption.  

This commands will completely wipe everything on **/dev/sdb**, including:  
- Partitions  
- Boot records  
- File systems  

#### SSD  
Use **blkdiscard (TRIM-based)** instead — it's fast and SSD-safe:  
```sh
sudo blkdiscard /dev/sdb
```  

## Reference
1. [Logical Volume Manager wiki](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux))
2. [mount](https://man7.org/linux/man-pages/man8/mount.8.html)
3. [LVM RedHat](https://www.redhat.com/en/blog/lvm-vs-partitioning)
4. [blkdiscard](https://man7.org/linux/man-pages/man8/blkdiscard.8.html)
[Master boot record(MBR) wiki](https://en.wikipedia.org/wiki/Master_boot_record)
[GUID Partition Table (GPT) wiki](https://en.wikipedia.org/wiki/GUID_Partition_Table)
[]()
[]()
