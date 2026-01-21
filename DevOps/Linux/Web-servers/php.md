# PHP
Install **PHP** on **CentOS**.  

## Install
Update repo and install php.  
```sh
sudo dnf update -y && \
sudo dnf install php php-mysqlnd -y
```

## Configure
Setup `/usr/local/lib/php.ini`.
```conf

```

## Apache
To use **php** on **httpd**, edit `/etc/httpd/conf/httpd.conf`.  
```conf
# Load the PHP 8 module
LoadModule php_module modules/libphp.so

# Parse certain extension as PHP
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>

# mod_rewrite
RewriteEngine On
RewriteRule (.*\.php)s$ $1 [H=application/x-httpd-php-source]
```

Restart **httpd**.  
```sh
sudo systemctl restart httpd
```


## Reference
1. [PHP Apache](https://www.php.net/manual/en/install.unix.apache2.php)
2. []()