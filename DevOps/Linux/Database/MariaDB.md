# MariaDB
Run **MariaDB** on **Linux**.  

# MariaDB on Linux

## Table of Contents
1. [Install MariaDB](#install-mariadb)  
2. [Initialize Database](#initialize-database)  
3. [Secure Installation](#secure-installation)  
4. [Run & Manage Service](#run--manage-service)  
5. [Configuration](#configuration)  
6. [Database & Users](#database--users)  
7. [Verify Installation](#verify-installation)  
8. [Reset Password](#reset-password)  
9. [Networking & Remote Access](#networking--remote-access)  
10. [Logs & Troubleshooting](#logs--troubleshooting)  
11. [Backup & Restore](#backup--restore)  
12. [Reference](#reference)  

---

## Install MariaDB
Update repo and install **MariaDB**.  
```sh
sudo dnf update -y && \
sudo dnf install mariadb-server -y
```

Create [runtime file](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s15.html) and set appropiate [permissions](../Users-and-ACL/README.md).  
```sh
sudo mkdir -p /var/run/mysqld
sudo chown mysql:mysql /var/run/mysqld # Assigns ownership of that directory to the mysql user and group.
```
**-p** - Ensures it doesn’t fail if it already exists.  

### Run & Manage Service
Enable, start, and check status of **MariaDB**.  
```sh
sudo systemctl enable mariadb && \
sudo systemctl start mariadb && \
sudo systemctl status mariadb
```

Restart:  
```sh
sudo systemctl restart mariadb
```

Stop:  
```sh
sudo systemctl stop mariadb
```

### Initialize Database
```sh
sudo mariadb-install-db --user=MySQL --basedir=/usr --datadir=/var/lib/mariadb
```

### Secure Installation
Enhance the security of your **MariaDB** installation and establish the **root** password.   
```sh
sudo mariadb-secure-installation
```
What it does:  
- Sets root password  
- Removes anonymous users  
- Disables remote root login  
- Removes test DB  

## Configuration
Main config file `/etc/my.cnf`.  

Key configs:  
```ini
[mysqld]
bind-address=127.0.0.1
port=3306
max_connections=200
```

Apply.  
```sh
sudo systemctl restart mariadb
```


## Database & Users
### Create Database
```sh
CREATE DATABASE kodekloud_db2;
```

### Create User + Grant Privileges  
```sh
GRANT ALL PRIVILEGES ON kodekloud_db2.* TO 'kodekloud_roy'@'%' IDENTIFIED BY 'dCV3szSGNA';
FLUSH PRIVILEGES;
```

### Verify installation
```sh
sudo mariadb -u root -p
```

Show databases.  
```sh
SHOW DATABASES;
```


### Verify
Verify by connecting to **MariaDB** using the new user.  
```sh
mariadb -u kodekloud_roy -p
```


## Reset Password
### Root User
Stop the database.  
```sh
sudo systemctl stop mariadb
```

Start MariaDB in safe mode.  
```sh
sudo mysqld_safe --skip-grant-tables --skip-networking &
```

Login without password
```sh
mariadb -u root
```

Reset the password  
```sql
FLUSH PRIVILEGES;

ALTER USER 'root'@'localhost'
IDENTIFIED BY 'NewStrongPassword';
```

Stop the safe server.  
```sh
sudo pkill mysqld
```

Start MariaDB normally.  
```sh
sudo systemctl start mariadb
```

Test login.  
```sh
mysql -u root -p
```

### Application User
Log in as the root(or any user with **CREATE USER / ALTER USER** privileges).  
While inside **MariaDB**, execute:  
```sh
ALTER USER 'super_user'@'localhost' IDENTIFIED BY 'new_strong_password';
FLUSH PRIVILEGES;
EXIT;
```

Test.  
```sh
mariadb -u super_user -p
```


## Networking & Remote Access
Edit config `/etc/my.cnf`.
```ini
bind-address=127.0.0.1 ## Localhost access
bind-address=0.0.0.0 # Public access
```

For public access, open the firewall.  
```sh
sudo firewall-cmd --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
```

## Reference
1. [MariaDB docs](https://mariadb.com/docs/server/mariadb-quickstart-guides/installing-mariadb-server-guide)
2. []()