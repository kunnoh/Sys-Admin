# Proxy
Create proxy using **SSH**, and use **Proxychains** to use **Socks5** ssh proxy address.  
Use **systemd** service to create persistent background running process, with easy control using systemd commands.  


## SSH
Create proxy using **SSH**.  
```sh
ssh -D 1080 -q -N user@hostname
```