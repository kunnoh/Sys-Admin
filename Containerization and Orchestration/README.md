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


Create a pod called time-check in the xfusion namespace. The pod should contain a container named time-check, utilizing the busybox image with the latest tag (specify as busybox:latest).

Create a config map named time-config with the data TIME_FREQ=10 in the same namespace.

Configure the time-check container to execute the command: while true; do date; sleep $TIME_FREQ;done. Ensure the result is written /opt/finance/time/time-check.log. Also, add an environmental variable TIME_FREQ in the container, fetching its value from the config map TIME_FREQ key.

Create a volume log-volume and mount it at /opt/finance/time within the container.

