# Arch Linux Packages

## Introduction  
Arch linux repo configuration.  
**journalctl** is a command-line utility in Linux used to view and manage logs stored by the systemd systemd-journald service.  


## Arch reflector
### Run as systemd service  
1. Create systemd service.  
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

2. Create the timer
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

3. Enable and start the timer.  
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


## Reference  
1. [Systemd wiki](https://en.wikipedia.org/wiki/Systemd)
2. [Systemd timers ArchWiki](https://wiki.archlinux.org/title/Systemd/Timers)
3. [Systemd service ArchWiki](https://wiki.archlinux.org/title/Systemd)
4. [Arch Linux packages](https://archlinux.org/packages/)
5. [Journal ArchWiki](https://wiki.archlinux.org/title/Systemd/Journal)
