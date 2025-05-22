Basic building block for Linux system.  First daemon serves as the root of use space's process tree.   



## EXAMPLE
**Tasks 1**
1. Create systemd service to run `/usr/bin/project-iko.sh` in background. 
2. Run python app after postgres DB. 
3. Use service account project_iko.
4. Auto restart on failure.
5. Restart interval 10 seconds.
6. Log service events.
7. Load when booting into graphical mode.


Create service file.
```sh
vim /etc/systemd/system/project-iko.sh
```

Run python app.
```conf
[Unit]
Description=python django app for project iko
Documentation=https://doc.iko.com
After=postgres.service

[service]
ExecStart=/usr/bin/project-iko.sh
User=project-iko
Restart=on-failure
RestartSec=10

[install]
WantedBy graphical.target
```


**Task 2**
Run reflector as a cronjob.  
Create systemd service.  
1. **Create systemd service**.  
    Create service file on `/etc/systemd/system/reflector-daily.service`.  
    
```conf
[Unit]
Description=Update Arch Linux mirrorlist daily using Reflector
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/reflector --age 12 --protocol https --sort rate --fastest 5 --save /etc/pacman.d/mirrorlist >> /var/log/reflector.log 2>&1 # Save log to /var/log/reflector.log
```  

2. **Create  the timer**.  
Create timer file on `/etc/systemd/system/reflector-daily.timer`.
```conf
[Unit]
Description=Run reflector daily

[Timer]
OnCalendar=daily # Daily run at midnight
Persistent=true # ensures that the task runs if the machine was off during the scheduled time

[Install]
WantedBy=timers.target
```  

3. **Enable and start the timer**.  
```sh
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now reflector-daily.timer
```

### Check status  
Check if `reflector-daily.service` is active:
```sh
systemctl status reflector-daily.service
```  

Check if `reflector-daily.timer` is active:
```sh
systemctl status reflector-daily.timer
```

See next run time:
```sh
systemctl list-timers | grep reflector
```

### Debugging  
Check `systemd` logs.  
```sh
journalctl -xeu reflector-daily.service
```

Check `reflector` logs.  
```sh
cat /var/log/reflector.log
```


## References
1. [Systemd Wiki](https://en.wikipedia.org/wiki/Systemd)
2. [Systemd.io](https://systemd.io/)
3. []()
