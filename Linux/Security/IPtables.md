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
**INPUT**  
INPUT rules govern traffic coming into the system.  
Example:  
```sh
iptables -I INPUT <other options>
```  

Allow `ssh` from from certain ip.  
```sh
iptables -I INPUT -p tcp -s 0.0.0.0/0 --dport 22 -j ACCEPT
iptables -A INPUT -j DROP
```
*Options*:  
- -I: Insert rule at the top of the INPUT chain.
- -A: Append rule at the bottom of the INPUT chain.
- -p: Protocol
- -s: Source IP
- --dport: Destination port
- -j: Action to take


**OUTPUT**  
OUTPUT rules govern traffic leaving the system.  
Example:  
```sh
iptables -I OUTPUT <other options>
```  


### Current rules  
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
- -D: delete rule  

### Persist rules.  
Save rules permenently.
- Debian:  
```sh
iptables-save > /etc/iptables/fules.v4
```  

- RHEL:  
```sh
service iptables save
```  

## Reference
1. [Redhat IPtables](https://www.redhat.com/en/blog/iptables)
2. [Controlling IP sets using IPtables](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/sec-setting_and_controlling_ip_sets_using_iptables#sec-Setting_and_Controlling_IP_sets_using_iptables)
