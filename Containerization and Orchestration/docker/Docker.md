# Docker

## Install
### Docker
Install docker in **RHEL**. 
```sh
yum-config-manager --add-repo [https://download.docker.com/linux/centos/docker-ce.repo](https://download.docker.com/linux/centos/docker-ce.repo)
```

### Docker-compose
```sh
curl -L "[https://github.com/docker/compose/releases/download/2.22.0/docker-compose-$(uname-s)-$(uname-m](https://github.com/docker/compose/releases/download/2.22.0/docker-compose-$(uname-s)-$(uname-m))" -o /usr/local/bin/docker-compose
```

### Docker MCP
Download binary and save to `/usr/local/lib/docker/cli-plugins/`.
```sh
curl -O --output-dir /tmp/ <url>
tar -xvf /tmp/<filename> /usr/local/lib/docker/cli-plugins/
```

## Configure
### Logs
Show last 40 lines in logs and follow log output.
```sh
docker logs --tail 40 -f <container name/id>
```

### Pull image and tag
Pull **busybox:musl** image and re-tag (create new tag) this image as **busybox:blog**.
```sh
docker pull busybox:musl
docker tag busybox:musl busybox:blog
```

### Copy files
copy an encrypted file `/tmp/kunta.txt.gpg` from docker host to `ubuntu_latest` container (running on same server) in `/tmp/` location. Please do not try to modify this file in any way.

Copy file.
```sh
docker cp /tmp/kunta.txt.gpg ubuntu_latest:/tmp
```

Verify file.
```sh
docker exec ubuntu_latest ls -la /tmp
```

### Create image from container
```sh
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
docker commit nginx_base my-custom-nginx:v1
```

### Docker cleanup  
Check the disk space used by the Docker daemon.   
```sh
docker system df
```

Clean up unused docker objects.   
```sh
docker system prune -a
```

Docker volume cleanup:
```sh
docker volume prune -af
```

## Debug Containers
Run debugging container, attach network or volume.
```sh
docker run --rm -it \
    -v <host_path>:<container_path> \
    --network <network_name> \
    debian:latest \
    bash
```



## Reference
1. [Docker docs]()
2. [Docker MCP](https://docs.docker.com/ai/mcp-catalog-and-toolkit/get-started/)