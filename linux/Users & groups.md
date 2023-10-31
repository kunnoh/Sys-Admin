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

validate
```sh
cat /etc/passwd | grep kun
```

#### User without home
- create a user `adn` without home directory
```sh
useradd -M adn
```
`-M` tells the useradd to create no home directory

- validate
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
move user files that belong to `kun` id on the folder `/home/usersdata` to `/ecommerce` and keep their file location.

```sh
ls -l /home/usersdata
```

use find to list file by user `kun`
```sh
find /home/usersdata/ -type f -user kun | wc -l
```

find and copy files to ecommerce folder
```sh
find /home/usersdata/ -type f -user kun -exec cp --parents {} /ecommerce \;
```

#### Disable root login
disable root login on ssh.
1. edit `/etc/ssh/sshd_config` .
2. change `PermitRootLogin yes` to `PermitRootLogin no` .
3. restart ssh daemon `systemctl restart sshd` 

#### Password-less SSH
use certifications keys for authentication.
- generate the needed keys. public and private keys.

```sh
ssh-keygen
```

- copy public key generated to the host you want to login to.

```sh
ssh-copy-id dere@kapp02
```

- now you try and login.

```sh
ssh dere@kapp02
```


#### Access control list
Check current permissions
```sh
getfacl /etc/hosts
```

Add group and user and other
```sh
setfacl --modify user::rwx,group::rwx,other::--- /var/www/html
```

**Remove access control entry**

The -x (remove) option can be used to remove access control entries.

```sh
setfacl -x group:admins:rwx /var/www/html
```

The -b or --remove-all option can be used to remove all access control entries from a file or directory.

```sh
setfacl -b /var/www/html
```

