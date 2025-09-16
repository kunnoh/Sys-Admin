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
### Mail User
Add user.  
```sh
sudo useradd -m -d /home/mark -s /bin/bash mark
```
Add password.  
```sh
sudo passwd mark
```

Verify.  
```sh
cat /etc/passwd | grep mark
```

### Postfix
Using **sed** and **grep**, show non-commented lines on `/etc/postfix/main.cf`.  
```sh
cat /etc/postfix/main.cf | grep -v '^#' | grep -v '^$'
```

Modify `/etc/postfix/main.cf`.  
```conf
myhostname = stratos.xfusioncorp.com
mydomain = xfusioncorp.com
myorigin = $mydomain
inet_interfaces = all

mynetworks = 127.0.0.0/8, 172.16.0.0/12
relay_domains = $mydestination
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
home_mailbox = Maildir/

# Security settings
smtpd_banner = $myhostname ESMTP
disable_vrfy_command = yes
smtpd_helo_required = yes

# Message size limits
message_size_limit = 10240000
mailbox_size_limit = 1024000000
```

Enable and start **Postfix**.  
```sh
sudo systemctl enable postfix
sudo systemctl start postfix
sudo systemctl status postfix
```

#### Test
Verify with **Telnet**.  
```sh
telnet stmail01 25
EHLO localhost
MAIL FROM: mark@stratos.xfusioncorp.com
RCPT TO: mark@stratos.xfusioncorp.com
DATA
Subject: Test Email
From: mark@stratos.xfusioncorp.com
To: mark@stratos.xfusioncorp.com

This is a test email.
.
QUIT
```



### Dovecot
Modify `/etc/dovecot/dovecot.conf`.
```conf
protocols = imap pop3 lmtp submission
listen = *, ::
```

Modify `/etc/dovecot/conf.d/10-mail.conf`  
```conf
mail_location = maildir:~/Maildir
```

Modify `/etc/dovecot/conf.d/10-auth.conf`.  
```conf
disable_plaintext_auth = yes
auth_mechanisms = plain login
```

Modify `/etc/dovecot/conf.d/10-master.conf`.  
```sh
auth-userdb {
    mode = 0666
    user = postfix
    group = postfix
}
```


Enable and start **Dovecot**.  
```sh
sudo systemctl enable dovecot
sudo systemctl start dovecot
sudo systemctl status dovecot
```

#### Test
Get email sent from **Postfix**
```sh
telnet localhost pop3
user mark
pass GyQkFRVNr3
retr 1 # retrieve 1st message
```


## Reference
1. [Postfix basic config](https://www.postfix.org/BASIC_CONFIGURATION_README.html)
2. [Dovecot config](https://doc.dovecot.org/2.4.1/core/config/quick.html)
[]()