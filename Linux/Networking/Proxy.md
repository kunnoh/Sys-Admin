# SSH SOCKS5 Proxy with Systemd Service
This guide shows how to create a **Socks5** proxy using **SSH**, **Proxychains** and **Tor**. Configure automatic and easy to manage using [systemd](https://systemd.io/) service.  

## SSH
### Server Side Configuration
On the remote server, edit `/etc/ssh/sshd_config`:
```sh
AllowTcpForwarding yes # Allow TCP forwarding on server
PermitOpen localhost:* 10.0.0.0/8:*  # Your IP
```
Refer setting [ssh configuration](../Security/SSH.md#ssh-configurations-and-hardening) on previous blog.

Restart **SSH** service.  
```sh
sudo systemctl restart sshd
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
Add or create `~/.ssh/config`:
```sh
Host mysocks
    HostName <server IP>
    User <server username>
    IdentityFile ~/.ssh/mykey
    DynamicForward 1080
    ServerAliveInterval 60
    ServerAliveCountMax 3
    Compression yes
    ExitOnForwardFailure yes
```
Refer to setting [ssh configuration](../Security/SSH.md#generate-ecdsa-key-pair-using-ssh-keygen) key pair.  

Ensure SSH key has correct permissions:  
```sh
chmod 600 ~/.ssh/mykey
```

Run **Socks5** proxy:
```sh
ssh -N -f mysocks
```

#### Verify
Check if the port is in use.  
```sh
sudo ss -tunlp | grep 1080
```

#### Test 
Check apparent IP, Should show server's IP address.  
```sh
curl --socks5 127.0.0.1:1080 https://ifconfig.me
```

Without proxy, shows your real IP address.  
```sh
curl https://ifconfig.me
```

## Systemd Service Setup
Create the service file `/etc/systemd/system/socks-proxy.service`:
```ini
[Unit]
Description=SSH SOCKS5 Proxy
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=<user>

ExecStart=/usr/bin/ssh -N -v mysock
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

# Security hardening
NoNewPrivileges=true
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

Manage the proxy service.  
```sh
sudo systemctl daemon-reload # Reload systemd to recognize the new service

sudo systemctl enable socks-proxy.service # Enable service to start on boot

sudo systemctl disable socks-proxy.service # Disable the service

sudo systemctl start socks-proxy.service # Start the service now

sudo systemctl status socks-proxy.service # Check service status

sudo systemctl restart ssh-socks-proxy # Restart the service

sudo systemctl stop socks-proxy.service # Stop the service
```

Logs.  
```sh
sudo journalctl -u ssh-socks-proxy -n 50 -f # View recent logs and follow
```


## Proxychains
Install **Proxychains**.  
```sh
sudo apt install proxychains # Debian-based
```

Configure proxy chains on `/etc/proxychains.conf`.  
```sh
dynamic_chain
proxy_dns
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.0/255.0.0.0
localnet 10.0.0.0/255.0.0.0
localnet 172.16.0.0/255.240.0.0
localnet 192.168.0.0/255.255.0.0
localnet ::1/128
kills_on_failure
chain_len = 1 # 1 proxy max

[ProxyList]
socks5  127.0.0.1 9050
socks5  127.0.0.1 1080

```

### Test
```sh
proxychains firefox google.com
proxychains curl https://ifconfig.me
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

## Reference
1. [SSH dynamic port forwarding](https://www.redhat.com/en/blog/ssh-dynamic-port-forwarding)  
2. [Proxychains](https://www.kali.org/tools/proxychains-ng/)  
3. [Systemd](https://www.freedesktop.org/software/systemd/man/latest/systemd.service.html)  
