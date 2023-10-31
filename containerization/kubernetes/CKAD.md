Am using `kind` to run kubernetes.
#### Installation
```sh
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
```

Add execution to binary
```sh
chmod +x ./kind
```

move `kind` to `/usr/local/bin`

```sh
mv ./kind /usr/local/bin/kind
```

check version
```sh
kind version
kind --version
```
#### Create cluster
create `mycluster` directory and add `mycluster.yaml`
```sh
vi ~/projects/mycluster/mycluster.yaml
```

add the following

```sh
kind: Cluster

apiVersion: kind.x-k8s.io/v1alpha4

nodes:

- role: control-plane

- role: worker

- role: worker
```

apply the cluster using `kind`

```sh
kind create cluster --name=mycluster --config=mycluster.yaml
```

confirm cluster deployment

```sh
kubectl get nodes
```

get component status. 
```sh
kubectl get cs
```

The `kubectl get cs` command is used to display the current status of the Control Plane components in a Kubernetes cluster.

get clusters using `kind`

```sh
kind get clusters
```

check cluster details

```sh
kubectl cluster-info --context kind-[cluster-name]
```
```sh
kubectl cluster-info --context kind-mycluster
```


#### Create dashboard for kubernetes


#### create pods in k8s cluster
Run apache pod in k8s
1. Check current status of pods
```sh
kubectl get pods
```

2. Create yaml file with all configs
```sh
vi httpd-pod.yaml
```
Add this configuration to file
```sh
apiVersion: v1
kind: Pod
metadata:
    name: pod-httpd
    labels:
      app: httpd_app
spec:
    containers:
    - name: httpd-container
      image: httpd:latest
```

3. Create pod
```sh
kubectl create -f httpd-pod.yaml
```

4. check the status of the pod
```sh
kubectl get pods -o wide
```

