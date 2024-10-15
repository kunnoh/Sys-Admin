- smallest object in kubernetes that can be created.
- can have multiple containers but not of the same kind.

Create pod.
```sh
kubectl run nginx --image nginx:latest
```

Get running pods.
```sh
kubectl get pods
```

or
```sh
kubectl get pods -o wide
```

Get more info on pod.
```sh
kubectl describe pod nginx
```


### Use yaml
Kubernetes follows top 4 fields that must be included on yaml file:
- apiVersion
- kind
- metadata
- spec
