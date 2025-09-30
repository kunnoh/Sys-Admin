# Kubernetes

## Get yaml of an object
```sh
kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml
```

## Template
### Namespace
```sh
kubectl create namespace <namespace-name> \
  --dry-run=client -o yaml > namespace.yaml
```

### Pod  
```sh
kubectl run <pod-name> \
  --image=<image-name> \
  --labels="app=my-app" \
  --restart=Never \
  --dry-run=client -o yaml > pod.yaml
```

### Deployment
```sh
kubectl create deployment <deployment-name> \
  --image=<image-name> \
  --namespace=<namespace-name> \
  --dry-run=client -o yaml > deployment.yaml
```

### NodePort
```sh
kubectl expose deployment <deployment-name> \
  --type=NodePort \
  --port=80 \
  --target-port=80 \
  --dry-run=client -o yaml > service.yaml
```

### 
