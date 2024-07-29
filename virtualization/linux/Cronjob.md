#### Users permission on cronjob
Allow user `peter` to add, edit and update cron job
- create `/etc/cron.allow` file and add the user to allow.
- add `peter`
```sh
peter
```

Deny user `mercy` to add, edit and update cron job
- create `cron.deny` on `/etc/cron.deny` 
- add `mercy`
```sh
mercy
```

Restart `crond` service 
```sh
systemctl restart crond.service
```

