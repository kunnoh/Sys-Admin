# Systemd
## Introduction
System and service manager for Linux acting as init system (PID 1).  
Basic building block for Linux system.  
First daemon serves as the root of use space's process tree.  
Bootstraps user space and manages system services. Responsible for initializing the system and launching other services and processes.

## Use cases
1. ### Django webapp
1. Create systemd service to run `/opt/project-iko/run.sh` in background. 
2. Run python app after postgres DB. 
3. Use service account project_iko.
4. Auto restart on failure.
5. Restart interval 10 seconds.
6. Log service events.
7. Load when booting into graphical mode.

#### Run.sh  
Create, make the script executable, start script file on `/opt/project-iko/run.sh` directory.
```sh
mkdir -p /opt/project-iko/
vi /opt/project-iko/run.sh
chmod +x /opt/project-iko/run.sh
ln -s /opt/project-iko/run.sh
```
Add:
```sh

```

#### Systemd service.  
Create service file on `/etc/systemd/system/` dir.
```sh
vi /etc/systemd/system/project-iko.service
```
Add:
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
WantedBy=graphical.target
```

#### Start and enable the project-iko.service
```sh
systemctl start project-iko.service # Start the service
systemctl enable project-iko.service # Run the app on restarts
```



2. ### Reflector with cronjob
**Reflector** - is a Python script which can retrieve the latest mirror list from the Arch Linux Mirror Status page, filter the most up-to-date mirrors, sort them by speed and overwrite the file /etc/pacman.d/mirrorlist. 

#### Systemd service  
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

#### Systemd timer  
Create systemd timer file on `/etc/systemd/system/reflector-daily.timer`.  
```conf
[Unit]
Description=Run reflector daily
Documentation=https://wiki.archlinux.org/title/Reflector

[Timer]
OnCalendar=daily # Daily run at midnight
Persistent=true # ensures that the task runs if the machine was off during the scheduled time

[Install]
WantedBy=timers.target
```  

#### Enable and start the timer
```sh
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now reflector-daily.timer
```

#### Status  
Check `reflector-daily.service` status:
```sh
systemctl status reflector-daily.service
```  

Check `reflector-daily.timer` status:
```sh
systemctl status reflector-daily.timer
```

See next run time for **systemd.timers**:
```sh
systemctl list-timers | grep reflector
```

#### Debugging  
Check `systemd` logs.  
```sh
journalctl -xeu reflector-daily.service
```
- **-x** - Add explanatory help text and metadata
- **-e** - Jump to the end (most recent entries)
- **-u** - Show logs for a specific unit (service)

Check `reflector` logs.  
```sh
cat /var/log/reflector.log
```



3. ### Arduino IDE with deesktop shortcut
Download arduino-ide appimage from official site.
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

#### Desktop entry
Create desktop config file on `/usr/share/applications/arduino-ide.desktop`.  
```ini
[Desktop Entry]
Version=2.3.6
Name=Arduino IDE
Comment=Open-source electronics prototyping platform
Exec=/usr/local/bin/arduino %F
Icon=/opt/arduino/resources/icons/512x512.png
Terminal=false
Type=Application
Categories=Development;Electronics;
MimeType=text/x-arduino;
StartupNotify=true
```

#### Permissions
Set proper permissions and update desktop applications.  
```sh
chmod 644 /usr/share/applications/arduino-ide.desktop
update-desktop-database /usr/share/applications
```

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
Reboot for group changes to take effect.  



## References
1. [Systemd Wiki](https://en.wikipedia.org/wiki/Systemd)
2. [Systemd.io](https://systemd.io/)
3. [Systemd timers](https://wiki.archlinux.org/title/Systemd/Timers)
3. [Reflector pacman](https://wiki.archlinux.org/title/Reflector)
4. [Arduino download](https://support.arduino.cc/hc/en-us/articles/360019833020-Download-and-install-Arduino-IDE)

