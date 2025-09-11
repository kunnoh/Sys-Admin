# Postfix Dovecot
Postfix as mail transfer agent.  
Dovecot as IMAP/POP3

## Installation
Install postfix and dovecot on Cent-OS 9.  
```sh
sudo dnf update -y &&\
sudo dnf install postfix dovecot -y
```

## Configuration
### Postfix
Modify `/etc/postfix/main.cf`,


### Dovecot
Add user
```sh
sudo useradd -d /home/rose -s /bin/bash rose
```
Add password.  
```sh
sudo passwd jim
```

Verify.  
```sh
cat /etc/passwd | grep jim
```

Telnet.  
```sh
telnet stmail01 25

EHLO

mail from:jim@stratos.xfusioncorp.com
rcpt to:jim@stratos.xfusioncorp.com
DATA
test mail
```


## Reference
1. [Postfix basic config](https://www.postfix.org/BASIC_CONFIGURATION_README.html)
2. [Dovecot config](https://doc.dovecot.org/2.4.1/core/config/quick.html)
[]()