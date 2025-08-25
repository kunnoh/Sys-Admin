# PHP-FPM Nginx
## Introduction
Run PHP-fpm using unix socket on as an upsstream server on nginx.

## Install PHP-FPM
### RHEL
#### Install EPEL Repository
```sh
sudo dnf install epel-release -y
```

#### Install Remi Repository
The Remi repository is a third-party software repository maintained by Remi Collet, a dedicated contributor to the RPM ecosystem.  
```sh
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

**Enable Remi Repository**
```sh
sudo dnf module reset php && \
sudo dnf module enable php:remi-8.3 -y
```

#### Install PHP-FPM 8.3
```sh
sudo dnf install php php-cli php-fpm php-mysqlnd php-xml php-mbstring php-json php-zip -y
```

##### Run as Unix Socket
Modify `/etc/php-fpm.d/www.conf`.  
```conf
Listen = /var/run/php-fpm/default.sock
```


### Nginx.  
```sh
sudo dnf update -y && \
sudo dnf install nginx
```

Add php-fpm as upstream server.  
```conf
        location ~ \.php$ {                                                          
          try_files $uri = 404;                                                      

          include fastcgi_params;                                                     

          fastcgi_pass  127.0.0.1:9000;                                              

          fastcgi_index index.php;                                      

          fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;        

         }                                                                
    location / {
        index  index.php index.html index.htm;                                       
    } 
```

```sh
sudo grep -v '^;' /etc/php-fpm.d/www.conf | grep -v '^$'
```


**Debian**
1. Update system and install **php-fpm**.
```sh
apt update && \
apt upgrade -y && \
apt install php-fpm
```

2. Install **PHP** extensions, **json, xml, mbstring, cURL** and **MySQL**.
```sh
apt install php-mysql php-curl php-json php-xml php-mbstring
```
- `php-mysql`: Connects PHP with MySQL databases.
- `php-curl`: Allows PHP to create HTTP requests.
- `php-json`: Enables PHP to handle JSON data.
- `php-xml`: Enables support for XML data.
- `php-mbstring`: Manages multi-byte strings.

## Configure PHP-FPM
1. Open the default PHP-FPM pool [www.conf](https://www.php.net/manual/en/install.fpm.configuration.php) configuration, `/etc/php/8.2/fpm/pool.d/www.conf`.
```ini
[www]
user = www-data
group = www-data
listen.owner = www-data
listen.group = www-data
listen = /run/php/php8.2-fpm.sock
```

2. Restart the PHP-FPM service to apply your configuration changes.
```sh
systemctl restart php8.2-fpm.service
```

## Reference
1. [php-config docs](https://www.php.net/manual/en/install.unix.debian.php)
2. [php-fpm config](https://www.php.net/manual/en/install.fpm.configuration.php)
2. []()

sudo systemctl status php8.2-fpm


---
# MariaDB
## Introduction

## Installation
```sh
apt install mariadb-server
```

Secure installation.
```sh
mysql_secure_installation
```

Create database and user.
```sql
CREATE DATABASE mbakapower;
CREATE USER 'wp_user'@'%' IDENTIFIED BY 'j7wJZmLWyebzCLZFp9qx';
GRANT ALL PRIVILEGES ON mbakapower.* TO 'wp_user'@'%';
FLUSH PRIVILEGES;
```


---
# WordPress

## Introduction

## Installation
Download wordpress.
```sh
wget https://wordpress.org/latest.zip
```

Grab a fresh set of salts from WordPress.
```sh
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```



## Reference
1. [MariaDB docs](https://mariadb.org/documentation/#getting-started)
2. [Wordpress docs](https://developer.wordpress.org/advanced-administration/before-install/howto-install/)
3. [PHP-FPM RHEL](https://infotechys.com/install-php-8-3-on-rhel-9-centos-9/)
[]()