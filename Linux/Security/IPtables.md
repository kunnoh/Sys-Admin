# IPtables

## Introduction
IPtables rules go into effect immediately after entering them.  
    Don't run both `firewalld` at the same time.  
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
Allow incoming traffic on ssh, http, https from any IP and Drop other traffic using policy.  
Allow all outgoing traffic.  
Allow localhost traffic.  

Allow `ssh` from from certain ip.  
```sh
iptables -I INPUT -p tcp -s <IP> --dport 22 -j ACCEPT
```  
Allow `http, https` from all IPs.  
```sh
iptables -A INPUT -p tcp --dport 80 -j ACCEPT && \
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```  
Allow localhost traffic.  
```sh
iptables -A INPUT -i lo -j ACCEPT
```  
Drop all incoming. Allow all outgoing.  
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
Save rules permenently.
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
