# Wireguard Debian 12

## Inroduction

**Enable ipv4 packet forwarding**
On `/etc/sysctl.conf`,
set `net.ipv4.ip_forward=1`

Apply changes.
```sh
sysctl -p
```

Check current settings.
```sh
sysctl -a
```

## Install and configure wireguard.
Update system.
```sh
apt update && apt upgrade -y && apt install wireguard
```

Generate keys both on server and client.
```sh
wg genkey | tee privatekey | wg pubkey > publickey
```

Create configuration file on the server.
```sh
sudo vim /etc/wireguard/wg0.conf
```

Add.
```conf
[Interface]
Address = 10.0.0.1/28
ListenPort = 51820

PrivateKey = <privateKey>
SaveConfig = true

PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o enX0 -j MASQUERADE;
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o enX0 -j MASQUERADE;
```

- Find network interface name.
```sh
ip -o -4 route show to default | awk '{print $5}'
```

Change file permission for `/etc/wireguard`.
```sh
chmod -R 600 /etc/wireguard/
```

Activate.
```sh
systemctl enable wg-quick@wg0
systemctl start wg-quick@wg0
```

Verify.
```sh
sudo wg
```


Client configuration
```conf
[Interface]  
Address = 10.0.0.2/8  
SaveConfig = true  
PrivateKey = <client_privatekey>  
  
[Peer]  
PublicKey = <server_publickey>  
AllowedIPs = 0.0.0.0/0  
Endpoint = <server_ip>:<server_wg_port>  
PersistentKeepalive = 30
```

Add peer to server.
```conf
[Peer]
PublicKey = 
AllowedIPs = 10.0.0.2/8
```

Restart the server `wg`.
```sh
systemctl restart wg-quick@wg0.service
```

## Reference
1. [Wireguard installation](https://www.wireguard.com/install)
2. [Wireguard configuration](https://www.wireguard.com/quickstart/)
