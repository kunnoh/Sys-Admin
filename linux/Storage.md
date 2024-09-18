Block devices - found under `/dev/` directory. Represent piece of hardware that can store data.
Data is written or read in block state.
To see block devices, use `lsblk`.
```sh
lsblk
```

or `ls` on `/dev/` 
```sh
ls -l /dev/ | grep "^b"
```

Major number:
- 1 - RAM
- 3 - HARD DISK or CD ROM
- 6 - PARALLEL PRINTERS
- 8 - SCSI DISK

**Disk partitions**
`fdisk` is used to list and partition disk.

List disks.
```sh
fdisk -l /dev/sda
```

Create new partition.
```sh
gdisk /dev/sda
```

type `n` to create new partition.
type `w` to write new partition.


**File systems**
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

#### EXT4
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

**Field**
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



### LVM
Logical volume manager - enables grouping multiple physical volume into logical volume.
Allows the disks to be resized dynamically.

**Create physical volume**
Use `pvcreate` to initialise a physical volume(PV) for use with lvm. Prepares storage device like a disk or partition to be used as part of a volume group(VG), which can then be dived into logical volumes(LVs).
```sh
pvcreate /dev/vdc
```

Verify.
```sh
pvs
```
or
```sh
pvdisplay
```

**Create volume group**
```sh
vgcreate iko_vg /dev/vdc
```

Verify.
```sh
vgdisplay
```


**Create logical volume**
```sh
lvcreate -L 5G -n vol1 iko_vg
```

Verify.
```sh
lvdisplay
```

Create filesystem.
```sh
mkfs.ext4 /dev/iko_vg/vol1
```


**Resize**
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