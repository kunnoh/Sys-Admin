## Install
- *debian, ubuntu*
```sh
sudo apt install ufw
```

**Verify**
```sh
sudo ufw version
```

## Configure
**Allow http, ssh and https**
```sh
sudo ufw allow 80 && 
sudo ufw allow 22 && 
sudo ufw allow 443
```

**Enable ufw**
```sh
sudo ufw enable
```

**Start ufw**
```sh
sudo ufw start
```

**Check status**
```sh
sudo ufw status
```