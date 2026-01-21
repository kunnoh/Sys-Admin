# PostgreSQL

## Installation
Install **PostgreSQL** on **CentOS**.  
```sh
dnf -y install postgresql-server postgresql-contrib
```

Initiate DB setup.  
```sh
postgresql-setup initdb
```

Start and enable **PostgreSQL**.  
```sh
systemctl start postgresql && systemctl enable postgresql
```


## Configuration
login to db using **psql**.  
```sh
psql -U postgres
```

Create user.  
```sh
CREATE ROLE kodekloud_sam WITH 
    LOGIN 
    PASSWORD 'Rc5C9EyvbU';
```

Create database.  
```sh
CREATE DATABASE kodekloud_db7;
```
Verify.  
```sh
\du # All users
\du kodekloud_sam # Single user info
```

Add privilleges to the user.  
```sh
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db7 TO kodekloud_sam;
```

Edit another config file `/var/lib/pgsql/data/pg_hba.conf` &  configure the md5.  
```conf
local   all    all                     md5
host    all    all   127.0.0.1/32      md5
host    all    all   ::1/128           md5
```

Verify.  
```sh
psql -U kodekloud_top -d kodekloud_db7 -h 127.0.0.1 -W
```