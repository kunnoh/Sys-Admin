# Shared Kubernetes Volume
```conf
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
    - name: share-logs
      emptyDir: {}
  containers:
    - image: nginx:latest
      name: nginx-container
      resources: {}
      volumeMounts:
        - name: share-logs
          mountPath: /var/log/nginx
    - image: ubuntu:latest
      name: sidecar-container
      command: ["sh", "-c", "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
      volumeMounts:
        - name: share-logs
          mountPath: /var/log/nginx
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```