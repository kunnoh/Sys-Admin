## User accounts
#### Create a user
create a user `kun` with userid `1200` .

```sh
adduser -u 1200 kun
```


#### Create user with no interactive shell
to create a user without interactive shell we use `adduser -s` to specify the shell to use.
```sh
adduser kun -s /bin/nologin
```

you can check by
```sh
cat /etc/passwd | grep kun
```


#### User without home
create a user `adn` without home directory
```sh
useradd -M adn
```
`-M` tells the useradd to create no home directory

check this by
```sh
cat /etc/passwd | grep adn
```


#### Create a group
create a group called `dev` .

```sh 
addgroup dev
```
add `kun` to the `dev` group.
```sh
usermod kun -G dev
```
#### Create user with expiry
create a user with name `alvin` which will be assisting kun. The user account should expire on `2023-06-22` .

```sh 
adduser alvin -e 2023-06-22
```

#### User files
move user files that belong to kun id on the folder /home/usersdata to /ecommerce and keep their file location.
List file ```ls -l /home/usersdata```

#### Disable root login
disable root login on ssh.
1. edit `/etc/ssh/sshd_config` .
2. change `PermitRootLogin yes` to `PermitRootLogin no` .
3. restart ssh daemon `systemctl restart sshd` .


#### Password-less SSH
use keys for authentication.
1. generate the needed keys. public and private keys.

```sh
ssh-keygen
```

2. copy public key generated to the host you want to login to.

```sh
ssh-copy-id dere@kapp02
```

3. now you try and login.

```sh
ssh dere@kapp02
```



#### Access control list
```sh
getfacl /etc/hosts
```

## Ops
#### create archives
Create a 
```sh
tar -czvf yousuf.tar.gz /data/yousuf/
```

#### set time
```sh
timedatectl
```

set new timezone
```sh
timectl set-timezone Africa/Nairobi
```






#### Substitute string
Get number of repetition of word `About` in the `nautilus.xml` documents.
```sh
cat /root/nautilus.xml  |grep About  | wc -l
```

Replace the word `About` with `ResCF`.
```sh
sed -i 's/About/ResCF/g' /root/nautilus.xml
```



#### Run levels

#### Cron job
Allow user `peter` to add, edit and update cron job
1. create `/etc/cron.allow` file and add the user to allow.

```sh
peter
```

Deny user `mercy` to add, edit and update cron job
2. create `cron.deny` on `/etc/cron.deny` 

```sh
mercy
```

Restart `crond` service 
```sh
systemctl restart crond.service
```



#### Set ntp service
Install `ntp` and add this server `server 1.sg.pool.ntp.org`
Check if `ntp` is installed
```sh
rpm -qa | grep ntp
```

Install `ntp`
```sh
yum install -y ntp
```

check if `ntp` wa installed
```sh
rpm -qa | grep ntp
```

Edit `/etc/ntp.conf` and add `server 1.sg.pool.ntp.org` to server session.

Enable `ntp.service` 

```sh
systemctl enable ntp.service
```

Start `ntp.service`
```sh
systemctl start ntp.service
```

Validate the task
```sh
ntpstat
```




#### Add firewalld
install firewalld
```sh
yum install firewalld -y
```

start firewalld service
```sh
systemctl start firewalld
```

enable firewalld service
```sh
systemctl enable firewalld
```

check status
```
systemctl status firewalld
```

check if `firewalld` is running
```sh
firewall-cmd --state
```

show ports or apps allowed
```sh
firewall-cmd --zone=public --list-all
firewall-cmd --zone=public --list-ports
```

Open all incoming connection on `3002/tcp` port. Zone should be `public`.
```sh
firewall-cmd --permanent --zone=public --add-port=3002/tcp
```

Reload firewalld
```sh
firewalld-cmd reload
```
reload `firewalld.service`
```sh
systemctl restart firewalld
```

Check if the port was added
```sh
firewalld-cmd --zone=public --list-all
```

```sh
firewalld-cmd --zone=public --list-ports
```

NB: [[https://www.rootusers.com/how-to-use-firewalld-rich-rules-and-zones-for-filtering-and-nat/]]

