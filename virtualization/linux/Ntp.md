#### Install ntp service
Install `ntp` and add this server `server 1.sg.pool.ntp.org`

- Check if `ntp` is installed
```sh
rpm -qa | grep ntp
```

- Install `ntp`
```sh
yum install -y ntp
```

- validate
```sh
rpm -qa | grep ntp
```

- Edit `/etc/ntp.conf` and add `server 1.sg.pool.ntp.org` to server session.

- Enable `ntp.service` 

```sh
systemctl enable ntp.service
```

- Start `ntp.service`
```sh
systemctl start ntp.service
```

- Validate the task
```sh
ntpstat
```


#### Set time
```sh
timedatectl
```

set new timezone
```sh
timectl set-timezone Africa/Nairobi
```
