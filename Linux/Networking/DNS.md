# Domain Name System
Serve's as the internet phonebook.  

Enable DNS over TLS and DNSSEC.  
Edit `/etc/systemd/resolved.conf`.  
```ini
[Resolve]
FallbackDNS=
DNSOverTLS=yes
DNSSEC=yes
Domain=~.
Cache=yes
```

Restart the services and check **resolved** status.  
```sh
sudo systemctl restart systemd-networkd
sudo systemctl restart systemd-resolved
```

Flush cache
```sh
sudo systemd-resolve --flush-caches
```

Test.  
```sh
sudo tcpdump -i any -n any -vvv -c 80 '(port 53 or port 853 or port 443)' > tcpdump_output_1.txt 2>&1
```

/etc/resolv.conf
/etc/hosts
/etc/nsswitch.conf