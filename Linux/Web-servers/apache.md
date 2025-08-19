# Apache/Httpd
Install and configure **Apache** on **CentOS**.  

## Installation
Update repo and install apache.  
```sh
sudo dnf update -y && \
sudo dnf install httpd -y
```


## Configure
Change `/etc/httpd/conf/httpd.conf` file and edit listen to port `6100`.
```conf

```

Start and enable **Apache**.  
```sh
sudo systemctl enable httpd && sudo systemctl start httpd && sudo systemctl status  httpd
```

