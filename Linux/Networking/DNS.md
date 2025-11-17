# Domain Name System
Serve's as the internet phonebook.  

## Set DNS
Wifi & Networking > SSID > IPv4 > Method Automatic (Only addresses).  
DNS servers:-  
    **Quad9**:  
        IPv4:  
            - 9.9.9.9  
            - 149.112.112.112  
        IPv6:  
            - 2620:fe::fe  
            - 2620:fe::9  
        **Features**:  
            - Built-in malware/phishing protection  
            - Privacy-focused/no logging  
            - DoH: https://dns.quad9.net/dns-query  
            - DoT: dns.quad9.net  
            - Supports DNSCrypt  
            - Supports DNSSEC validation  
            - NXDOMAIN for blocked malicious domains  
        **Trade-offs**:  
            - Slow  
            - May block legitimate sites  
            - Less global infra  
    **Cloudflare**:  
        IPv4:  
            - 1.1.1.1  
            - 1.0.0.1  
        IPv6:  
            - 2606:4700:4700::1111  
            - 2606:4700:4700::1001  
        **Features**:  
            - DoH: https://cloudflare-dns.com/dns-query  
            - DoT: cloudflare-dns.com  
            - Strong privacy. Dont log IP addresses  
            - Fast - matches Google  
            - Suports DNSSEC validation  
            - Standard NXDOMAIN responses  
            - Filtering variants available (1.1.1.2, 1.1.1.3)  
        **Trade-offs**:  
            - Privacy requires trust  
            - Limited customization  
            - Not fastest  
    **Google**:  
        IPv4:  
            - 8.8.8.8  
            - 8.8.4.4  
        IPv6:  
            - 2001:4860:4860::8888  
            - 2001:4860:4860::8844  
        **Features**:  
            - DoH: https://dns.google/dns-query  
            - DoT: dns.google  
            - Suports DNSSEC validation  
            - Standard NXDOMAIN responses  
            - Excellent uptime, realibility and performance  
        **Trade-offs**:  
            - Logs queries  
            - Privacy concerns  
            - No malware protections  

DOT - Secure tunnel port 853  
DNSSEC -  
DoH -   

### Configure DoT and DNSSEC.  
Edit `/etc/systemd/resolved.conf`.  
```ini
[Resolve]
# DNS Servers
FallbackDNS=1.1.1.1#cloudflare-dns.com 2606:4700:4700::1111#cloudflare-dns.com 8.8.8.8#dns.google 2001:4860:4860::8888#dns.google

# Security Settings
DNSSEC=yes
DNSOverTLS=yes
MulticastDNS=no
LLMNR=no
Domains=~.# Route all domains through encrypted DNS
DNSOverTLS=opportunistic # Alternative: Use 'yes' for strict (may break some networks)
ResolveUnicastSingleLabel=no # Already set - prevents single-label DNS queries
DNSStubListenerExtra= # Can add additional stub listeners if needed

# Performance Optimizations
Cache=yes
CacheFromLocalhost=no
DNSStubListener=yes
ReadEtcHosts=yes
ResolveUnicastSingleLabel=no
StaleRetentionSec=3600
CacheTTLSec=0 # Respect upstream TTL (0=use server TTL)
NegativeCacheTTLSec=10 # Cache negative responses for 10 seconds (default: 120)

# Network Optimization
Protocols=dns https-dns # Prioritize DoT and DoH if available
DNSDefaultRoute=yes # Use this resolver as default route for all domains

# Advanced Tuning
MaxCacheEntries=8192 # Increase cache size (default: 4096) - more memory, faster lookups
```  

Restart the services and check **resolved** status.  
```sh
sudo systemctl restart systemd-networkd
sudo systemctl restart systemd-resolved
```  

Flush cache.  
```sh
sudo systemd-resolve --flush-caches
```  

Test.  
```sh
sudo tcpdump -i any -n any -vvv -c 80 '(port 53 or port 853 or port 443)' > tcpdump_output_1.txt 2>&1
```  

### Monitor
Monitor DNS queries in real-time.  
```sh
sudo resolvectl monitor
```  

Check DNS statistics.  
```sh
sudo resolvectl statistics
```  

/etc/resolv.conf  
/etc/hosts  
/etc/nsswitch.conf  

## Reference
1. [DNS Wiki](https://en.wikipedia.org/wiki/Domain_Name_Systemhttps://en.wikipedia.org/wiki/Domain_Name_System)  
2. [DNS security](https://www.cloudflare.com/learning/dns/dns-security/)  