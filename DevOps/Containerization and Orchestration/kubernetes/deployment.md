# Kubernetes Deployment

## Deployment
Get deployment yaml.  
```sh
kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml
```
Modify `/tmp/nginx.yaml` Force changes.  
```sh
kubectl replace -f /tmp/nginx.yaml --force
```

## Rollout
Check status.
```sh
kubectl rollout status deployment/<deployment_name>
```

List history of deployment.  
```sh
kubectl rollout history deployment/<deployment_name>
```

Upgrade deployment.  
```sh
kubectl set image deployment/<your-deployment-name> <container-name>=httpd:2.4.43
```

Undo deployment.
```sh
kubectl rollout undo deployment/<deployment_name> --to-revision=<revision_number>
```


## Reference
1. [Kubernetes rollout](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_rollout/)
