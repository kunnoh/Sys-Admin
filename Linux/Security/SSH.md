# SSH Configuration

## Introduction  
The **Secure Shell Protocol (SSH Protocol)** is a cryptographic network protocol for operating network services securely over an unsecured network. Its most notable applications are remote login and command-line execution.

**OpenSSL** is a cryptographic library that enables an open source implementation of **Transport Layer Security (TLS)** and **Secure Sockets Layer (SSL)** protocols. It provides functions to generate private keys, manage certificates, and equip client applications with encryption and decryption.  

## Generate SSH key pair 
**OpenSSL** is a software library for applications that provide secure communications over computer networks against eavesdropping, and identify the party at the other end.  

In cryptography, the **Elliptic Curve Digital Signature Algorithm (ECDSA)** offers a variant of the **Digital Signature Algorithm (DSA)** which uses elliptic-curve cryptography. 

**ECDSA** is one of the more complex public key cryptography encryption algorithms.  


### Generate ECDSA key pair using OpenSSL  
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


Copy Public key to the server  
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
- `etc/ssh/sshd_config` - SSH server. How others connect to you.
- `etc/ssh/ssh_config`  - SSH client. How you connect to others.


### ssh_config
Add remote host on `~/.ssh/config`.  
```sh
Host <host>
HostName <hostname/ip addr>
Port <ssh port>
User <server user>
IdentityFile <private key file>
```

### sshd_config
Complete [/etc/ssh/sshd_config](./sshd) file with ssh hardening.  

Validate.  
```sh
sshd -T
```

#### Key improvements
1. Strong cryptography: Moder cipher suites.  
2. Reduced attack surface: Disabled unnecessary features i.e TCP forwarding, agent forwarding.  
3. Strictier authentication: reduced login grace time and max auth tries.  
4. Connection management: Better idle timeout settings.  
5. Algorithm whitlisting: Using modern secure and MAC algorithms.  
6. Disabled password authentication: Using key pair to authenticate.  
7. Disabled root authentication: To login as a user you must login as an allowed user then use sudo to login as a root user.
8. Whitelisted users and groups to login: Only certain users and groups can login.  


Restart **SSH** service.  
```sh
sudo systemctl daemon-reload && \
sudo systemctl restart ssh
```

#### Recommendations
1. Change SSH port - to reduce automated attacks.  
2. Install and configure fail2ban.  

## Reference
1. [SSH wiki](https://en.wikipedia.org/wiki/Secure_Shell)
2. [OpenSSL wiki](https://en.wikipedia.org/wiki/OpenSSL)
3. [ECDSA wiki](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)  
4. [Encryption Algorithm](https://www.encryptionconsulting.com/education-center/what-is-an-encryption-algorithm/)  
5. [ECDSA encryptionconsulting](https://www.encryptionconsulting.com/education-center/what-is-ecdsa/)  
6. [OpenSSH](https://www.openssh.com/)
