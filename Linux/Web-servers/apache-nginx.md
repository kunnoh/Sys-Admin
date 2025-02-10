First you need to install apache
```sh
yum install httpd
```

Configure it on port as given.
For that you need to change `/etc/httpd/conf/httpd.conf` file and edit listen parameter.
Restart the httpd service.

For nginx, you need to install nginx on server using,
```sh 
yum install epel-release
```
```sh
yum install nginx
```

then you need to configure some parameter in `nginx.conf` located at `/etc/nginx`
Edit the file and change below parameters according to your question.
```sh
listen 80996;
listen [::]:8096;
server_name 172.16.238.16;
```

Under location tab, add below line
```sh
proxy_pass http://72.16.238.16:5003
```

test new config
```sh
nginx -t
```

Start the nginx service.
```sh
systemctl reload nginx
```

Check the web page by using curl command
```sh
curl http://72.16.238.16:5003
```
