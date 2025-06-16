# Systemd
## Introduction
System and service manager for Linux acting as init system (PID 1).  
Basic building block for Linux system.  
First daemon serves as the root of use space's process tree.  
Bootstraps user space and manages system services. Responsible for initializing the system and launching other services and processes.

## Use cases
### Run Django webapp
1. Create systemd service to run `/usr/bin/project-iko.sh` in background. 
2. Run python app after postgres DB. 
3. Use service account project_iko.
4. Auto restart on failure.
5. Restart interval 10 seconds.
6. Log service events.
7. Load when booting into graphical mode.


#### Create systemd service.  
Create service file on `/etc/systemd/system/` dir.
```sh
vim /etc/systemd/system/project-iko.sh
```
add:
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


### Run reflector daily with cronjob
**Reflector** - is a Python script which can retrieve the latest mirror list from the Arch Linux Mirror Status page, filter the most up-to-date mirrors, sort them by speed and overwrite the file /etc/pacman.d/mirrorlist. 

#### Create systemd service  
Create service file on `/etc/systemd/system/reflector-daily.service` to run daily cron job.  
Add this:  
```conf
[Unit]
Description=Update Arch Linux mirrorlist daily using Reflector
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/reflector --age 12 --protocol https --sort rate --fastest 5 --save /etc/pacman.d/mirrorlist >> /var/log/reflector.log 2>&1 # Save log to /var/log/reflector.log
```  

#### Create systemd timer  
Create systemd timer file on `/etc/systemd/system/reflector-daily.timer`.  
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

### Add arduino IDE
Download arduino-ide appimage.
```sh
wget https://support.arduino.cc/hc/en-us/articles/360019833020-Download-and-install-Arduino-IDE
```

Make arduino executable then, move to `/opt/arduino` directory and create symbolic link to `/usr/local/bin/arduino`.  
```sh
chmod +x arduino-ide*
mkdir -p /opt/arduino/
mv arduino-ide*.AppImage /opt/arduino/arduino-ide
ln -s /opt/arduino/arduino-ide /usr/local/bin/arduino
```

#### Create systemd service.  


Setup **USB** permissions.
```sh
usermod -aG dialout,lock,uucp $USER
```

Create udev rules for arduino devices.
```sh
vim /etc/udev/rules.d/99-arduino.rules
```
Add this:
```conf
# Arduino boards
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0043", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0001", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0010", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0243", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0001", MODE="0666"

# CH340/CH341 (common on Arduino clones)
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="5523", MODE="0666"

# FTDI (used on some Arduino boards)
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", MODE="0666"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6015", MODE="0666"
```

Apply changes.
```sh
# Reload udev rules
sudo udevadm control --reload-rules
sudo udevadm trigger
```
Log out and log back in (or reboot) for group changes to take effect.  



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
3. [Reflector arch linux](https://wiki.archlinux.org/title/Reflector)
4. [Arduino download](https://support.arduino.cc/hc/en-us/articles/360019833020-Download-and-install-Arduino-IDE)

