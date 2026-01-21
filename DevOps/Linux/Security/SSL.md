# SSL
## OpenSSL(Self-Signed Certificate)
### Generate 

```sh
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```


# 1. Generate private key
openssl genrsa -out private-key.pem 4096

# 2. Generate certificate signing request (CSR)
openssl req -new -key private-key.pem -out csr.pem

# 3. Generate self-signed certificate
```sh
openssl x509 -req -in csr.pem -signkey private-key.pem -out certificate.pem -days 365
```
