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


## Reference
1. [Nginx docs](https://nginx.org/en/docs/)
