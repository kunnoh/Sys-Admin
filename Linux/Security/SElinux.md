# SELinux
Linux kernel security module.  

## Introduction
SELinux provides mechanism for ssupporting access contril security policies.

## Installation
**RHEL**
```sh
yum install selinux* -y
```

- check `selinux` status
```sh
sestatus
```

Edit the `/etc/selinux/config` and add 
```sh
SELINUX=disabled
```

## Reference
1. [RedHat SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
2. []()