# PHP-FPM
## Introduction

## Installation
**Debian**
1. Update system and install **php-fpm**.
```sh
apt update
apt upgrade -y
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
systemctl restart php8.2-fpm
```

## Reference
1. [php-config docs](https://www.php.net/manual/en/install.unix.debian.php)
2. [php-fpm config](https://www.php.net/manual/en/install.fpm.configuration.php)
2. []()

sudo systemctl status php8.2-fpm