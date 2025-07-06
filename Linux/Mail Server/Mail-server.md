# Linux Mail Server
## Introduction
A mail server relies on 3 protocols:
1. **SMTP** (Simple Mail Transfer Protocol) - handles sending emails which makes it act as digistal postal service.
2. **POP3** (Post Office Protocol) - manages email retrieval working as a personal mailbox handlers.
3. **IMAP** (Internet Message Access Prorocol) - manages email retrieval working as a personal mailbox handlers.

### SMTP Server
Handles routing and delivery of every mail that leaves the server.
Eamples:
    - Sendmail - pioneered email delivery in Unix systems.
    - Postfix - modern choice with good reasons. Keeps all processes different.


## Mail Server
### Components
Mail server contains 3 major components **MUA**, **MTA**, and **MDA**.
- **Mail User Agent** is the frontend interface where users interact with email, i.e compose and read emails. Can be **Roundcube** or **Thunderbird**.
- **Mail Transport Agent** does routing of emails to their destinations. eg **Postfix**.
- **Mail Delivery Agent** works as a mail sorter, processing incoming emails and placing them in right inbox. Applies filters.

A typical email flow in the systems:
    1. Write an email in your MUA
    2. MTA picks it up and finds the best route
    3. The recipient’s MDA processes and delivers it
    4. Recipient MUA displays the message


### Setting Up
Postfix acts like an organized mailroom. It has all mail server components.

#### Step 1: Install Postfix

#### Step 2: Configure Settings

#### Step 3: Restricks networks

#### Step 4: Test


### Configure
#### Configure Parameters
Open `/etc/postfix/main.cf` and set this:
```conf
myhostname = mail.yourdomain.com  
mydomain = yourdomain.com  
myorigin = $mydomain  
mydestination = $myhostname, localhost.$mydomain, $mydomain  
mynetworks = 127.0.0.0/8 [::1]/128  
inet_protocols = all  
mail_spool_directory = /var/spool/mail
```

##### Security First

##### Storage Optimization

##### Test
Reload postfix service
```sh
sudo systemctl reload postfix
```

Check errors;
```sh
tail -f /var/log/mail.log
```

### Test Mail Server
#### Step: Verify DNS Records

#### Step: Send Test Email
#### Step: Check Delivery
#### Step: 


### Secure Mail Server
#### SMTP User Authentication
##### Enable SASL Authentication

#### SSL/TLS Encryption
#### Configure Mail Relay
#### SPF: Sender Policy Framework
#### DKIM: DomainKeys Identified Mail



## Reference
1. [Contabo mail server](https://contabo.com/blog/linux-mail-server-setup-and-configuration/)