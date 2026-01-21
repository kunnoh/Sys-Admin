# IPtables

## Introduction
IPtables rules go into effect immediately after entering them.  
Don't run **FirewallD** and **IPtables** simultaneously.  
The order of rules matters.  

## Installation
Update and install **IPtables**.  
```sh
sudo apt update && sudo apt install iptables
```

## Rules  
**Basic formula for iptables (and nftables) is**:  
```sh
iptables -I <chain> 
[ -s <source ip> / -i <input ethernet interface> ] 
[ -d <destination ip> / -o <output ethernet interface> ] 
[ -p tcp / udp / icmp --dport / --sport <port> ] 
-j ACCEPT / DENY / DROP
```
Accept is obvious, deny is “return message immediately saying not allowed” and drop is “silently drop packets, pretending there’s no service running on that port.”


**Rules to know:**  
```sh
iptables -L -n -v --line-number
iptables -A <args> # append at the end of a chain, like -A INPUT
iptables -I <args> # insert at the top of a chain, like -I INPUT
iptables -I 4 # insert at line 4
iptables -D <args> # delete rule
iptables -D 4 # delete rule 4
iptables-save > /a/file
iptables-restore /a/file
iptables -I FORWARD -s <source> -d <destination> -m state --state RELATED,ESTABLISHED -j ACCEPT # mostly used for routing, but might have some uses if you block all input to a server; what this means is basically "allow connections that were established from destination initially to come in from a source which normally gets blocked by a deny-all rule"
```

**OUTPUT**  
OUTPUT rules govern traffic leaving the system.  
Example:  
```sh
iptables -I OUTPUT <other options>
```  
**INPUT**  
INPUT rules govern traffic coming into the system.  
Example:  
```sh
iptables -I INPUT <other options>
```  

### Use case  
Allow incoming traffic on ssh from specific IP with rate limit, http, https from any IP and Drop other traffic using policy.  
Allow all outgoing traffic.  
Allow localhost traffic.  
Allow ICMP for monitoring.  

1. Allow `ssh` with rate limit from certain ip.  
```sh
iptables -A INPUT -p tcp -s <IP> --dport 22 -m limit --limit 3/min --limit-burst 5 -j ACCEPT
```  
2. Allow `http` and `https` from all IPs.  
```sh
iptables -A INPUT -p tcp --dport 80 -j ACCEPT && \
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```  
3. Allow `ICMP` traffic from all IPs.  
```sh
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```  
4. Allow localhost traffic.  
```sh
iptables -A INPUT -i lo -j ACCEPT
```  
5. Drop all incoming traffic, allow all outgoing traffic using policy.  
```sh
iptables -P INPUT DROP && \
iptables -P FORWARD DROP && \
iptables -P OUTPUT ACCEPT
```  
*Options*:  
- -I: Insert rule at the top of the INPUT chain.
- -A: Append rule at the bottom of the INPUT chain.
- -p: Protocol
- -P: Policy
- -s: Source IP
- --dport: Destination port
- -j: Action to take  

**Single command:**
```sh
iptables -A INPUT -i lo -j ACCEPT && \
iptables -A INPUT -p tcp --dport 80 -j ACCEPT && \
iptables -A INPUT -p tcp --dport 443 -j ACCEPT && \
iptables -A INPUT -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT && \
iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT && \
iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min --limit-burst 5 -j ACCEPT && \
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT && \
iptables -P INPUT DROP && \
iptables -P FORWARD DROP && \
iptables -P OUTPUT ACCEPT && \
iptables -A INPUT -j LOG --log-prefix "Dropped Packet: " --log-level 4
```  

### Check rules  
Current `iptables` rules.  
```sh
iptables -L -v -n --line-numbers
```  
*Options*:  
- -L: List current firewall rules.  
- -v: Verbose mode. show extra details like packet/byte counters & interface names.  
- -n: Numeric output. Prevent DNS lookups for IP addresses.  
- --line-numbers: Show line numbers. Useful for modifying and deleting specific rule.  

### Delete the rule.  
```sh
iptables -D INPUT 1
```  
*Options*:  
- -D: delete rule by number.  

### Persist rules.  
Save rules permanently.
- Debian:  
```sh
iptables-save | tee /etc/iptables/rules.v4
```  

- RHEL:  
```sh
service iptables save
```  




We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8087 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:


1. Install iptables and all its dependencies on each app host.

2. Block incoming port 8087 on all apps for everyone except for LBR host.

3. Make sure the rules remain, even after system reboot.

```sh
sudo iptables -I INPUT -p tcp -m tcp --dport 8081 -j ACCEPT  && sudo iptables-save > /etc/sysconfig/iptables &&  cat /etc/sysconfig/iptables
sudo iptables -I INPUT -p tcp -m tcp --dport 3002 -s 172.16.238.14 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 3002 -j DROP
sudo iptables -R INPUT 5 -p icmp -j REJECT
```

## Reference
1. [Redhat IPtables](https://www.redhat.com/en/blog/iptables)
2. [Controlling IP sets using IPtables](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/sec-setting_and_controlling_ip_sets_using_iptables#sec-Setting_and_Controlling_IP_sets_using_iptables)
3. [Learn Linux](https://community.learnlinux.tv/t/ufw-or-firewalld/3311/6)
