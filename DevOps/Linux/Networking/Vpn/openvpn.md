# OpenVpn
### Access Server
**Installation**

*debian/ubuntu*. 
1. Download install file and save on `/tmp`.
```sh
wget https://as-repository.openvpn.net/as/install.sh -O /tmp/openvpn-install.sh
```

2. Change permissions.
```sh
sudo chmod +x /tmp/openvpn-install.sh
```

3. Run `install.sh` non-interactively and save output. This log will contain username and password.
```sh
DEBIAN_FRONTEND=noninteractive yes | sudo /tmp/openvpn-install.sh >> output-$(date).log
```

4. Access server using public ip address. For admin use `ip-address/admin`.
