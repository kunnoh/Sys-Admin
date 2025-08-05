## Table of content

## Introduction
Containers and orchestration.  

Create cronjob template.  
```sh
kubectl create cronjob cronjob-name --image=image-name --schedule="* * * * *" --dry-run=client -o yaml > cronjob.yaml
```

Create job template.  
```sh
kubectl create job job-name --image=image-name --dry-run=client -o yaml -- bash -c echo "Hello world" > job.yaml
```