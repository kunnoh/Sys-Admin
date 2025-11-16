# Proxy
Create proxy using **SSH**, and use **Proxychains** to use **Socks5** ssh proxy address.  
Use **systemd** service to create persistent background running process, with easy control using systemd commands.  


## SSH
### Server Side Configuration
On `/etc/ssh/sshd_config`, add.
```sh
AllowTcpForwarding yes
PermitOpen localhost:* 10.0.0.0/8:*  # Optionally restrict destinations
```

### Client Side Configuration
Create proxy using **SSH**.  
```sh
ssh -D 1080 -f -N -C myusername@11.11.11.11
```
**Options**:
- **`-D`** - Dynamic port forwarding.  
- **`-f`** - Background process.  
- **`-N`** - No remote commends (just forwarding).  
- **`-C`** - Enable compression, for slow connections.  

#### Persistent Configuration
Add to `~/.ssh/config`:
```sh
Host mysocks
    HostName 11.11.11.11 
    User myusername
    IdentityFile ~/.ssh/mykey
    DynamicForward 1080
    ServerAliveInterval 60
    ServerAliveCountMax 3
    Compression yes
```
Refer to setting [ssh configuration](../Security/SSH.md) key pair.  
Run **Socks5** proxy:
```sh
ssh -N -f mysocks
```

#### Test 
Check apperent IP, Should show server's IP address.  
```sh
curl --socks5 127.0.0.1:1080 https://ifconfig.me
```

Without proxy, shows your real IP address.  
```sh
curl https://ifconfig.me
```

#### Stop the Proxy
Check the process running on port 1080.  
```sh
sudo ss -tulnp | grep '1080'
```
take the pid and kill the process.  
```sh
sudo kill -9 <pid>
```

## Proxychains
Install **Proxychains**.  
Configure proxy chains on `/etc/proxychains.conf`.  
```sh
dynamic_chain
quiet_mode
proxy_dns
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000

[ProxyList]
socks5 127.0.0.1 9050
socks5 127.0.0.1 1080
```

## Reference
1. [SSH dynamic port forwarding](https://www.redhat.com/en/blog/ssh-dynamic-port-forwarding)  