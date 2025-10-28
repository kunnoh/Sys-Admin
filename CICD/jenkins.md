# Jenkins

## Installation
### CentOS 9
Use [Fedora](https://www.jenkins.io/doc/book/installing/linux/#fedora).  
```sh
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf upgrade
# Add required dependencies for the jenkins package
sudo dnf install fontconfig java-21-openjdk
sudo dnf install jenkins
sudo systemctl daemon-reload
```

```sh
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

## Configure
### User
Add user.  

### Hosts
1. Add SSH plugin, SSH Credentials Plugin, SSH Build Agents plugin Version.  
   Click ***Jenkins > Manage Jenkins > Plugins***.  
   Install **SSH, SSH-build-agent**.  
2. Create SSH key pair on deployment server.  
3. On jenkins dashboard, add credentials.  
   Click ***Manage Jenkins > Credentials***
4. Add SSH hosts on jenkins dashboard.  
   Click ***Jenkins > Manage Jenkins > System#SSH remote hosts***   

## Build job
Install a job to install a package on the production server.  
1. 


## Reference
1. [Jenkins docs](https://www.jenkins.io/doc/book/installing/linux/)