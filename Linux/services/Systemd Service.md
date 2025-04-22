**Tasks**
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

