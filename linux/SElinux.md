#### SELinux
- Install `SELinux`
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

