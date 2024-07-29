## Set mikrotik haplite as repeater using ssh

login to the router using ssh
```sh
ssh admin@192.168.88.1
```

this method causes an error

`Unable to negotiate with 192.168.88.1 port 22: no matching host key type found. Their offer: ssh-dss,ssh-rsa`

you should add the option `-oHostKeyAlgorithms=+ssh-dss` to the SSH command:

```sh
ssh -oHostKeyAlgorithms=+ssh-dss admin@192.168.88.1
```

**check**

