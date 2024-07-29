## Installation
proxmox can be installed on debian or as a bare metal.

**Bare metal**
installed using USB bootable iso.
1. download proxmox iso. Current version `proxmox VE 8`.
```sh
wget -c 
```

2. create bootable usb drive
```sh
dd if=proxmox of=/dev/sdb bs=1M status=progress && sync
```
