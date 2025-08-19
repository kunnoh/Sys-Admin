# MariaDB
Install **MariaDB** on **CentOS**.  

## Install
Update repo and install **MariaDB**.  
```sh
sudo dnf update -y && \
sudo dnf install mariadb-server -y
```

Enable and start **MariaDB**.  
```sh
sudo systemctl enable mariadb && \
sudo systemctl start mariadb && \
sudo systemctl status  mariadb
```

For security, run security script that comes pre-installed with **MariaDB**.  
```sh
sudo mysql_secure_installation
```

Verify installation by connecting as root:Bash
```sh
sudo mariadb -u root -p
```

Show databases while logged in to **MariaDB**.  
```sh
SHOW DATABASES;
```


## Configure
Create database.  
```sh
CREATE DATABASE kodekloud_db9;
```

Create user and grant database privileges to the user.  
```sh
GRANT ALL ON kodekloud_db9.* TO 'kodekloud_joy'@'localhost' IDENTIFIED BY 'B4zNgHA7Ya' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

Verify by connecting to **MariaDB** using the new user.  
```sh
sudo mariadb -u kodekloud_joy -p
```

Show databases.  
```sh
SHOW DATABASES;
```



## Reference
1. [MariaDB docs](https://mariadb.com/docs/server/mariadb-quickstart-guides/installing-mariadb-server-guide)
2. []()