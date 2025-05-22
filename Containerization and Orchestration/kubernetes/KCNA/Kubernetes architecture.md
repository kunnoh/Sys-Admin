- **Node** - machine in which kubernetes is installed. It's a worker machine in which containers will be launched.
- **Master** -  machine in which kubernetes is installed and configured as master. It watches the nodes and is responsible for actual orchestration of containers in node.
- **Cluster** - group of nodes.

## Kubernetes Components
- **Api-server** - act as front-end for for kubernetes.

- **Controller** - Brain behind orchestration. Responsible for responding to nodes or container when they go down.

- **Container runtime** - underlying software that's used to run containers. 

- **Kubelet** - agent that runs on each node in the cluster. Make sure each container is running.

- **Scheduler** - distributing workload on container between the nodes. Looks for newly created containers and assigns them to nodes.

- **Etcd** - distributed key-value store for storing data used to manage cluster. Generate logs to ensure no conflicts between the masters.


### Container Runtime Interface
Enables any container vendor to work as a container runtime for kubernetes as long as they adhere to Open Container Initiative or OCI.
Defines gRPC protocol to connect to container runtimes.

#### Open Container Initiative (OCI)
Consists of :-
	- imagespec - specification on how the image should be built.
	- runtimespec - specification on how container runtime should be developed.


- Docker
- rkt
- ContainerD

**ContainerD**
CLI - ctr
- not user friendly.
- supports limited features.

CLI - nerdctl
- provides a docker like CLI for containerD.
- supports docker-compose.
- supports newest features in containerd.
	- Encrypted container images.
	- Lazy pulling.
	- P2P image distribution.
	- Image signing and verifying.
	- Namespaces in kubernetes.

CLI - crictl
- provides CLI for CRI compatible runtimes.
- installed separately.
- used to inspect and debug container runtimes.
- works across different runtimes.
#### CRI-O
