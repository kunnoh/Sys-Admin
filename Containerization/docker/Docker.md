#### Install docker
install docker in rhel
```sh
yum-config-manager --add-repo [https://download.docker.com/linux/centos/docker-ce.repo](https://download.docker.com/linux/centos/docker-ce.repo)
```


#### Install docker-compose
```sh
curl -L "[https://github.com/docker/compose/releases/download/2.22.0/docker-compose-$(uname-s)-$(uname-m](https://github.com/docker/compose/releases/download/2.22.0/docker-compose-$(uname-s)-$(uname-m))" -o /usr/local/bin/docker-compose
```



#### Docker copy files
copy an encrypted file `/tmp/kunta.txt.gpg` from docker host to `ubuntu_latest` container (running on same server) in `/tmp/` location. Please do not try to modify this file in any way.

Copy file.
```sh
docker cp /tmp/kunta.txt.gpg ubuntu_latest:/tmp
```

Verify file.
```sh
docker exec ubuntu_latest ls -la /tmp
```


#### Webapp with Docker
There is a static website running within a container named `kunta`, this container is running on `App Server 1`. Suddenly, we started facing some issues with the static website on `App Server 1`. Look into the issue to fix the same, you can find more details below:  
1. Container's volume `/usr/local/apache2/htdocs` is mapped with the host's volume `/var/www/html`.  
2. The website should run on host port `8080` on `App Server 1` i.e command `curl http://localhost:8080/` should work on `App Server 1`.



#### Docker cleanup  
Docker system cleanup:
```sh
docker system df
docker system prune -a
```

Docker volume cleanup:
```sh
docker volume prune -af
```
