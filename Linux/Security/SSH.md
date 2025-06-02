# SSH Configuration

## Introduction  
The **Secure Shell Protocol (SSH Protocol)** is a cryptographic network protocol for operating network services securely over an unsecured network. Its most notable applications are remote login and command-line execution.

**OpenSSL** is a cryptographic library that enables an open source implementation of **Transport Layer Security (TLS)** and **Secure Sockets Layer (SSL)** protocols. It provides functions to generate private keys, manage certificates, and equip client applications with encryption and decryption.  

## Generate SSH key pair 
**OpenSSL** is a software library for applications that provide secure communications over computer networks against eavesdropping, and identify the party at the other end.  

In cryptography, the **Elliptic Curve Digital Signature Algorithm (ECDSA)** offers a variant of the **Digital Signature Algorithm (DSA)** which uses elliptic-curve cryptography. 

**ECDSA** is one of the more complex public key cryptography encryption algorithms.  


### Generate ECDSA key pair uisng OpenSSL  
1. Generate private key.  

    ```sh
    openssl ecparam -genkey -name prime256v1 -noout -out /path/to/ecdsa_private_key_name.pem
    ```

2. Generate public key.  

    ```sh
    openssl ec -in /path/to/ecdsa_private_key_name.pem -pubout -out /path/to/ecdsa_public_key_name.pem
    ```

3. Verify generated keys.  

    ```sh
    openssl ec -in /path/to/ecdsa_private_key.pem -check
    ``` 

4. Convert Public Key to OpenSSH format.  
    Servers that require public key to be in `OpenSSH` format for authentication.  

    ```sh
    ssh-keygen -f /path/to/ecdsa_public_key_name.pem -i -m PKCS8 > /path/to/public_key_openssh.pub
    ``` 

5. Change permission to read for owner.  

    ```sh
    chmod 400 /path/to/ecdsa_private_key_name.pem
    ```  

### Generate ECDSA key pair using ssh-keygen
Gnerate key pair.  
```sh
ssh-keygen -t ed25519 -C "your key comment" -f /path/to/key/filename
```  
- `-t` - Specify algorithm to use when generating the key pair e.g `-t rsa`, `-t ecd25519`.    
- `-C` - Add comment to the key pair.  
- `-f` - Specify output folder and name.  


### Copy Public key to the server  
**Automatically**  
Use `ssh-copy-id` to copy the public key. This will ask for password being used currently.  
```sh
ssh-copy-id -i /path/to/your/key.pub user@server_ip
```  

**Manually**  
1. Copy the generated **public key** on the host, i.e where it was generated.  
```sh
cat /path/to/ecdsa_public_key_openssh.pub
```  

2. Ssh to the server.  
- Create `~/.ssh` directory if it doesn't exist on home directory.  
```sh
mkdir ~/.ssh && chmod 600 -R ~/.ssh/
```  
- Append copied public key from local host to the server on `~/.ssh/authorized_keys`. **~/.ssh/authorized_keys** file must be owned by the user.  


## SSH configurations and hardening  
**Config files:**
- `etc/ssh/sshd_config` - SSH srver. How others connect to you.
- `etc/ssh/ssh_config`  - SSH client. How you connect to others.


1. Complete `sshd_config` file with ssh hardening.  
```conf
Include /etc/ssh/sshd_config.d/*.conf

Port 22
AddressFamily any
ListenAddress 0.0.0.0
ListenAddress ::

UsePAM yes # Needed for advanced features like user resource limits, 2FA or sessions limits, and system policies.
PrintMotd no
AcceptEnv LANG LC_* # Send locale variables
Subsystem       sftp    /usr/lib/openssh/sftp-server

# Authentication
PubkeyAuthentication yes
PasswordAuthentication no
PermitRootLogin prohibit-password
PermitEmptyPasswords no
LoginGraceTime 320
MaxAuthTries 3
MaxSessions 4
AuthorizedKeysFile .ssh/authorized_keys
KbdInteractiveAuthentication no # Disables keyboard-interactive auth. Reduce brute-force or 2FA bypass vectors.
ChallengeResponseAuthentication no # Challenge-response authentication (used for one-time passwords or legacy systems). Often paired with UsePAM yes to manage authentication
IgnoreRhosts yes # Don't read the user's ~/.rhosts and ~/.shosts files

# Kerberos options
#KerberosAuthentication no
#KerberosOrLocalPasswd yes
#KerberosTicketCleanup yes
#KerberosGetAFSToken no

# GSSAPI options
GSSAPIAuthentication no
#GSSAPICleanupCredentials yes
#GSSAPIStrictAcceptorCheck yes
#GSSAPIKeyExchange no

X11Forwarding no
AllowTcpForwarding no
PermitUserEnvironment no
ClientAliveInterval 300
ClientAliveCountMax 2
AllowUsers <youruser>

# Logging
LogLevel VERBOSE
# SyslogFacility AUTH

Banner /etc/issue.net # Display a custom banner i.e for legal notices
Compression yes # Improve speed on slow links
```


## Reference
1. [SSH wiki](https://en.wikipedia.org/wiki/Secure_Shell)
2. [OpenSSL wiki](https://en.wikipedia.org/wiki/OpenSSL)
3. [ECDSA wiki](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)  
4. [Encryption Algorithm](https://www.encryptionconsulting.com/education-center/what-is-an-encryption-algorithm/)  
5. [ECDSA encryptionconsulting](https://www.encryptionconsulting.com/education-center/what-is-ecdsa/)  
6. [OpenSSH](https://www.openssh.com/)
