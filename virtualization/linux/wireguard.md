### Installation
```sh
sudo apt update
sudo apt install wireguard
```

### Generate key pair
```sh
wg genkey | sudo tee /etc/wireguard/private.key
```
```sh
sudo chmod go=/etc/wireguard/private.key
```

`sudo chmod go=...` removes any perms on the file for users and groups other than the root user to ensure only root can access the file.

The next step is to create the corresponding public key, which is derived from the private key. Use the following command to create the public key file:

```sh
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
```

- `sudo cat /etc/wireguard/private.key`: this command reads the private key file and outputs it to the [standard output](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection#standard-output) stream.
- `wg pubkey`: the second command takes the output from the first command as its [standard input](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-i-o-redirection#standard-input) and processes it to generate a public key.
- `sudo tee /etc/wireguard/public.key`: the final command takes the output of the public key generation command and redirects it into the file named `/etc/wireguard/public.key`.


### Choosing addresses
The WireGuard Server will use a single IP address from the range for its private tunnel IPv4 address. We’ll use `10.8.0.1/24` here, but any address in the range of `10.8.0.1` to `10.8.0.255` can be used. Make a note of the IP address that you choose if you use something different from `10.8.0.1/24`. You will add this IPv4 address to the configuration file that you define in [Creating a WireGuard Server Configuration](https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-debian-11#step-3-%E2%80%94-creating-a-wireguard-server-configuration).