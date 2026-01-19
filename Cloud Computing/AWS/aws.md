# Amazon Web Service

## Reset password on EC2
1. Login to cloud console and stop running EC2 vm.  
2. Search **volumes** add detach the root EBS volume from the stopped instance.  
3. Create a new EC2 and choose existing block storage as the previous EC2 storage.  
4. Search **volumes** add and attach the detached root volume to the helper instance as a secondary volume (e.g., /dev/sdf or /dev/xvdf).  
SSH into the helper instance.  
5. Run `lsblk` to show attached volume, mount it.  
6. Create a new key  
```sh
ssh-keygen -t ed25519
```
7. Add **public key** to the detached volume on `/home/.ssh/authorized_keys`.
8. Unmount the disk and terminate the EC2.  
```sh
sudo umount /mnt/recover
```
9. Detach the mounted volume and mount back to original EC2. Restart the EC2.