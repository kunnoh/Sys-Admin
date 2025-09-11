# Postfix Dovecot
Postfix as mail transfer agent.  
Dovecot as IMAP/POP3

## Installation
Install **Postfix** and **Dovecot** on **CentOS 9**.  
```sh
sudo dnf update -y &&\
sudo dnf install postfix dovecot -y
```

## Configuration
#### Mail User
Add user.  
```sh
sudo useradd -m -d /home/jim -s /bin/bash jim
```
Add password.  
```sh
sudo passwd jim
```
Verify.  
```sh
cat /etc/passwd | grep jim
```

### Postfix
Modify `/etc/postfix/main.cf`.  
```conf
myhostname = stmail01.stratos.xfusioncorp.com
mydomain = stratos.xfusioncorp.com
myorigin = $mydomain
inet_interfaces = all
```

Enable and start **Postfix**.  
```sh
sudo systemctl enable postfix
sudo systemctl start postfix
sudo systemctl status postfix
```

Verify with **Telnet**.  
```sh
telnet stmail01 25

EHLO

mail from:jim@stratos.xfusioncorp.com
rcpt to:jim@stratos.xfusioncorp.com
DATA
test mail
```


### Dovecot


#### Create email account
Create the Maildir directory.  
```sh
sudo maildirmake /home/rose/Maildir
```

Edit `/etc/dovecot/dovecot.conf`.
```conf
protocols = imap pop3 lmtp
listen = *, ::
mail_location = maildir:~/Maildir
userdb {
    driver = passwd
}
passdb {
    args = %s
    driver = pam
}
```

Enable and start **Dovecot**.  
```sh
sudo systemctl enable dovecot
sudo systemctl start dovecot
sudo systemctl status dovecot
```


## Reference
1. [Postfix basic config](https://www.postfix.org/BASIC_CONFIGURATION_README.html)
2. [Dovecot config](https://doc.dovecot.org/2.4.1/core/config/quick.html)
[]()