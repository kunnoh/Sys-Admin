# Kubernetes

## Template
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