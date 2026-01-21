# Proxychains Tor

## Installation
Install **Proxychains** and **Tor**.  
```sh
sudo pacman -S proxychains tor
```

## Configure
Modify `/etc/proxychains.conf`.
```conf
strict_chain
quiet_mode

[ProxyList]
socks5 127.0.0.1 9050 # Use Tor proxy
```