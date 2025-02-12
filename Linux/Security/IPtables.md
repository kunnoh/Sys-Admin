# IPtables

## Introduction
IPtables rules go into effect immediately after entering them.  
Don't run `firewalld` and `iptables` simultaneously.  
The order of rules matters.  

## Installation
```sh
apt update && apt install iptables
```

## Rules  
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

## Reference
1. [Redhat IPtables](https://www.redhat.com/en/blog/iptables)
2. [Controlling IP sets using IPtables](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/sec-setting_and_controlling_ip_sets_using_iptables#sec-Setting_and_Controlling_IP_sets_using_iptables)
