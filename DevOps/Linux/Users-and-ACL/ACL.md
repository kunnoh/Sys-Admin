# Access Control List
Set user and group.  
```sh
sudo setfacl -m u:vivek:r /etc/resolv.conf
sudo setfacl -m g:devops:rw /etc/resolv.conf
```

Verify.  
```sh
getfacl /etc/resolv.conf
```