#### Attack vectors
- source code repository
- artifact repository
- chart repository
- container registry

#### Securing container images
- grant minimum privileges. without user instruction, container run as root.
- set setuid & setguid. setuid bit on causes process to run with permissions of file owner.
- scan in CI pipeline after building the image.
- scan image registry.
- 

#### Securing host & container running environment

```sh
docker run --privileged myImage
```

- privileged container pierce the wall of security between OS and container.
- implement network policies.
	- bridge
	- special: host, none
	- underlay networks
	- overlay networks
- do not mount docker daemon inside a container.
- create new bridge and attach only containers that need to communicate.
- accept traffic from specific host
```sh
docker run -p 10.5.7.8:4554:8080 webserver
```

#### Securing Kubernetes
- isolation among pods.
- access control for applications.
- pod network security.
- secret management.
- misconfigurations
- communications among core components.
- access management.
- 

