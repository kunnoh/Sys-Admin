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

#### Expose deployment
```sh
kubectl expose deployment <deployment-name> \
  --type=NodePort \
  --port=80 \
  --target-port=80 \
  --dry-run=client -o yaml > service.yaml
```

### NodePort
```sh
kubectl create service nodeport <service-name> \
  --tcp=80:80 \
  --node-port=30008 \
  --dry-run=client -o yaml > service.yaml
```

### Persistent Volume
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes: [ReadWriteOnce]
  hostPath:
    path: /data
```

### Persistent Volume Claim
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: manual
  resources:
    requests:
      storage: 1Gi
```
