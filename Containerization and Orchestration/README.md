## Table of content

## Introduction
Containers and orchestration.  

Create cronjob template.  
```sh
kubectl create cronjob <cronjob-name> --image=<image-name> --schedule="<cron-schedule>" --dry-run=client -o yaml > cronjob.yaml
```

Create job template.  
```sh
kubectl create job my-job --image=busybox -- date
```
