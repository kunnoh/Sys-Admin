### Install  
```sh
apt update && apt install firewalld -y
```

### Enable & Start  
Enable firewalld service to run when system boots.  
```sh
systemctl enable firewalld
```  

Start firewalld service.  
```sh
systemctl start firewalld
```  

Check status of `firewalld service`.  
```
systemctl status firewalld
```  

Check if `firewalld` is running.
```sh  
firewall-cmd --state
```

### Configure firewalld  
Show apps allowed.  
```sh
firewall-cmd --zone=public --list-all
```  

Show ports allowed.  
```
firewall-cmd --zone=public --list-ports
```  

Open all incoming connection on `3002/tcp` port. Zone should be `public`.  
```sh
firewall-cmd --permanent --zone=public --add-port=3002/tcp
```  

Reload `firewalld.service`
```sh
systemctl restart firewalld
```

Verify.  
```sh
firewalld-cmd --zone=public --list-all
```  


## Reference
 1. [FirewallD rootusers.com](https://www.rootusers.com/how-to-use-firewalld-rich-rules-and-zones-for-filtering-and-nat/)
