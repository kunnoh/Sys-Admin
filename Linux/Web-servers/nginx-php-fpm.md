# PHP-FPM Nginx
## Overview
Setup nginx and php-fpm on **CentOS 9**. Configure php-fpm as an upstream server on nginx using unix socket on `/var/run/php-fpm/default.sock`.

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
sudo dnf module enable php:remi-8.1 -y
```

#### Install PHP-FPM 8.3
```sh
sudo dnf install php php-cli php-fpm php-mysqlnd php-xml php-mbstring php-json php-zip -y
```

##### Run as Unix Socket
Create dir and change permissions.  
```sh
sudo mkdir /var/run/php-fpm && \
sudo chown nginx:nginx /var/run/php-fpm && \
sudo chmod 755 /var/run/php-fpm
```

Show uncommented lines on `/etc/php-fpm.d/www.conf`.  
```sh
sudo grep -v '^;' /etc/php-fpm.d/www.conf | grep -v '^$'
```

Modify `/etc/php-fpm.d/www.conf`.  
```conf
[www]
user = nginx
group = nginx

; Unix socket
listen = /var/run/php-fpm/default.sock

; Set permissions for the socket
listen.owner = nginx
listen.group = nginx
listen.mode = 0660

; Process manager settings
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
```


## Install Nginx.  
```sh
sudo dnf update -y && \
sudo dnf install nginx -y
```

Add php-fpm as upstream server.  
```conf
server {
    listen 80;
    
    server_name <example.com>;

    root /var/www/html;
    index index.php index.html index.htm;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;

    # Handle PHP files
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Handle static files
    location / {
        try_files $uri $uri/ =404;
    }

    # Deny access to hidden files
    location ~ /\. {
        deny all;
    }

    # Optimize static content
    location ~* \.(jpg|jpeg|gif|png|css|js|ico|xml)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```


## Run the system
Start nginx and php-fpm.  
```sh
sudo systemctl start nginx php-fpm
```

## Reference
1. [MariaDB docs](https://mariadb.org/documentation/#getting-started)
2. [PHP-FPM RHEL](https://infotechys.com/install-php-8-3-on-rhel-9-centos-9/)
[]()
3. []()