# SELinux
Linux kernel security module.  

## Introduction
SELinux provides mechanism for ssupporting access contril security policies.

## Installation
**RHEL**
```sh
yum install selinux* -y
sudo yum install -y policycoreutils selinux-policy-targeted libselinux-utils
```

- check `selinux` status
```sh
sestatus
```

Edit the `/etc/selinux/config` and add 
```sh
SELINUX=disabled
```

Permanentl disble 
```sh
sudo sed -i 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
sudo sed -i 's/^SELINUX=permissive/SELINUX=disabled/' /etc/selinux/config
```

## Reference
1. [RedHat SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
2. []()