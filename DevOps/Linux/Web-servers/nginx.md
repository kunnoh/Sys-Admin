# Nginx

## Installation
**Debian**
1. Install the [prerequisites](https://nginx.org/en/linux_packages.html#Debian):
```sh
sudo apt install curl gnupg2 ca-certificates lsb-release debian-archive-keyring
```

2. Import an official nginx signing key so apt could verify the packages authenticity. Fetch the key:
```sh
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
| sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

3. To set up the apt repository for stable nginx packages, run the following command:
```sh
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
http://nginx.org/packages/debian `lsb_release -cs` nginx" \
| sudo tee /etc/apt/sources.list.d/nginx.list
```

4. Install nginx, run the following commands:
```sh
sudo apt update
sudo apt install nginx
```

5. Enable nginx. Auto restart on boot.
```sh
systemctl enable nginx
```








You're on nginx 1.24.0, which is the version Ubuntu 24.04 (Noble) ships by default. To get a newer version, you need to switch to the official nginx.org repository — Ubuntu's repos won't move past 1.24.0.
Latest versions available

    Stable: 1.30.4 (recommended for production)
    Mainline: 1.31.4 (newer features, more frequent updates)

Step-by-step upgrade
1. Backup your config
bash

sudo cp -r /etc/nginx /etc/nginx.backup.$(date +%Y%m%d)

2. Install prerequisites
bash

sudo apt update
sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring

3. Add the official nginx signing key
bash

curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null

4. Add the nginx.org repository
For stable (recommended):
bash

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
https://nginx.org/packages/ubuntu $(lsb_release -cs) nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list

For mainline (latest features):
bash

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
https://nginx.org/packages/mainline/ubuntu $(lsb_release -cs) nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list

5. Pin nginx.org packages higher than Ubuntu's
bash

echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx

6. Update and upgrade nginx
bash

sudo apt update
sudo apt install nginx

7. Verify
bash

nginx -v

8. Test config and reload
bash

sudo nginx -t
sudo systemctl reload nginx









## Reference
1. [Nginx docs](https://nginx.org/en/docs/)
