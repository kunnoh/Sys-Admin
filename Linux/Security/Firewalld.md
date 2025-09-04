# Firewalld
Firewalld provides a dynamically managed firewall with support for network/firewall zones that define the trust level of network connections or interfaces.

## Install  
```sh
sudo apt update && sudo apt install firewalld -y
```

### Run FirewallD  
Start and enable firewalld service to run when system boots.  
```sh
sudo systemctl enable firewalld 
sudo systemctl start firewalld
```  

Check **firewalld** status.  
```sh
sudo systemctl status firewalld
sudo firewall-cmd --state
```

## Configure  
Show services and ports allowed for a zone.  
```sh
sudo firewall-cmd --zone=public --list-all
sudo firewall-cmd --get-services # get all active services
```  

### Zone
Create a zone.  
```sh
sudo firewall-cmd --new-zone=public --permanent
```

Set created zone as default.  
```sh
sudo firewall-cmd --set-default-zone=public --permanent
```

Add interface to zone.  
```sh
sudo firewall-cmd --get-zone-of-interface=eth0 --permanent
sudo firewall-cmd --zone=<zone> --add-interface=eth0 --permanent
```

Allow access to aport from specific subnet/IP.  
```sh
# Allow access to ssh from 192.168.0.12 sing IP address
sudo firewall-cmd --add-rich-rule 'rule family="ipv4" service name="ssh" \
source address="192.168.0.12/32" accept' --permanent

# Allow access to ssh from 10.1.1.0/24 network
sudo firewall-cmd --add-rich-rule 'rule family="ipv4" service name="ssh" \
source address="10.1.1.0/24" accept' --permanent
```

List rich rules on the system.  
```sh
sudo firewall-cmd --list-rich-rules
```

Enable port forwarding:
```sh
# Enable masquerading
sudo firewall-cmd --add-masquerade --permanent

# Port forward to a different port within same server ( 22 > 2022)
sudo firewall-cmd --add-forward-port=port=22:proto=tcp:toport=2022 --permanent

# Port forward to same port on a different server (local:22 > 192.168.2.10:22)
sudo firewall-cmd --add-forward-port=port=22:proto=tcp:toaddr=192.168.2.10 --permanent

# Port forward to different port on a different server (local:7071 > 10.50.142.37:9071)
sudo firewall-cmd --add-forward-port=port=7071:proto=tcp:toport=9071:toaddr=10.50.142.37 --permanent
```

### Rules
#### Add
Open all incoming connection on **http https ssh** port. Zone should be **public**.  
```sh
# by service
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
#or
sudo firewall-cmd --permanent --zone=public --add-service={http,https}


# by port
sudo firewall-cmd --permanent --zone=public --add-port={22/tcp,80/tcp,443/tcp}

# Restart FirewallD
sudo systemctl restart firewalld
```  
The `--permanent` option means persist rules against server reboots.


#### Delete
To remove rules.  
```sh
# by service
sudo firewall-cmd --permanent --zone=public --remove-service={http,https}

# by port
sudo firewall-cmd --permanent --zone=public --remove-port={80/tcp,443/tcp}

# Restart
sudo systemctl restart firewalld
```


### Docker
FirewallD conficts with docker Iptables.  
```sh
sudo firewall-cmd --zone=public --add-masquerade --permanent
sudo firewall-cmd --reload
sudo systemctl restart docker
```

## Reference
1. [FirewallD](https://firewalld.org/)
2. [FirewallD rootusers.com](https://www.rootusers.com/how-to-use-firewalld-rich-rules-and-zones-for-filtering-and-nat/)
3. [Docker IPtables](https://docs.docker.com/engine/network/packet-filtering-firewalls/)