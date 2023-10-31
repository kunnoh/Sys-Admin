#### Cron job
Allow user `peter` to add, edit and update cron job
1. create `/etc/cron.allow` file and add the user to allow.

```sh
peter
```

Deny user `mercy` to add, edit and update cron job
2. create `cron.deny` on `/etc/cron.deny` 

```sh
mercy
```

Restart `crond` service 
```sh
systemctl restart crond.service
```

