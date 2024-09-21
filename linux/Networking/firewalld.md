#### Install firewalld
- download `firewalld` from yum
```sh
yum install firewalld -y
```

#### Run firewalld
- enable firewalld service to run when system boots
```sh
systemctl enable firewalld
```

- start firewalld service
```sh
systemctl start firewalld
```

- check status of `firewalld service`
```
systemctl status firewalld
```

- check if `firewalld` is running
```sh
firewall-cmd --state
```


#### Configure firewalld
- show apps allowed
```sh
firewall-cmd --zone=public --list-all
```

- show ports allowed
```
firewall-cmd --zone=public --list-ports
```

- Open all incoming connection on `3002/tcp` port. Zone should be `public`.
```sh
firewall-cmd --permanent --zone=public --add-port=3002/tcp
```

- Reload firewalld
```sh
firewalld-cmd reload
```

- reload `firewalld.service`
```sh
systemctl restart firewalld
```

- Check if the port was added
```sh
firewalld-cmd --zone=public --list-all
```

- check ports
```sh
firewalld-cmd --zone=public --list-ports
```


## Reference
 1. [FirewallD rootusers.com](https://www.rootusers.com/how-to-use-firewalld-rich-rules-and-zones-for-filtering-and-nat/)
