# Setup Ubuntu
using Ubuntu to setup new production server.

## Table of Content
1. [Create User](#user)
2. [SSH Hardening](#SSH)
[]()

## User
### Create user
```sh
adduser dev
```

### User Permissions
Add `dev` user to `sudo` group.  
```sh
adduser dev sudo
```

#### Passwordless
Check rule into `/etc/sudoers.d/` using **visudo**.  
Then user that needs **sudo** access **WITH** a password:
```sh
adduser dev sudo
```

User that needs **sudo** access **WITH NO** password:
```sh
%dev ALL=(ALL) NOPASSWD: ALL
```

Verify that user can successfully escalate privileges.  
```sh
sudo -i
```

### Generate SSH Key
Generate `id255` key on the local machine.  
```sh
ssh-keygen -t ed25519 -f ~/.ssh/dev_ed25519
```

### Authorise Key
Add `~/.ssh/dev_ed25519.pub` to `~/.ssh/authorized_keys` on the server so you can log in as the dev user without a password.  
```sh
ssh-copy-id ops@192.168.1.10
```
**Altanetively:**
Copy **public key** from the local machine to `~/.ssh/authorized_keys` on the server.  

### Use Key to Signin
Drop a small config override file into `/etc/ssh/sshd_config.d/` instead of editing the main sshd config at `/etc/ssh/sshd_config`.

Add `/etc/ssh/sshd_config.d/auth`.  
```sh
PermitRootLogin no # no root login
PubkeyAuthentication yes # enable key authentication
PasswordAuthentication no # disable password authentication
```

### Verify Configuration
```sh
sudo systemctl reload ssh
```

Restart SSH using modified configuration on `/etc/ssh/sshd_config`.  
```sh
sudo systemctl restart sshd
```

### Root Password
Exit to root user and lock password.  
```sh
exit
passwd -l root
```

To keep a usable root password, rotate it to a new strong secret with `passwd root` instead.


## SSH Hardening

