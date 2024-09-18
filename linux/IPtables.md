#### INPUT
Accept rule.
```sh
iptables -A INPUT -p tcp -s 10.0.8.3 --dport 22 -j ACCEPT
```
- -A: Add rule
- -p: protocol
- -s: source
- -d: destination
- --dport: destination port
- -j: action to take
- -I: add to top
- -D: delete rule

#### OUTPUT
