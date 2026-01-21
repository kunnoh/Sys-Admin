## Install
- *debian, ubuntu*
```sh
apt update && apt install ufw
```

**Verify**
```sh
ufw version
```

## Configure
**Allow http, ssh and https**
```sh
ufw allow 80 && 
ufw allow 22 && 
ufw allow 443
```

**Enable ufw**
```sh
ufw enable
```

**Start ufw**
```sh
ufw start
```

**Check status**
```sh
ufw status
```