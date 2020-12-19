---
title: "Home Server"
date: 2020-12-18T17:45:30-05:00
draft: false
author: Mike Quigley
---

I've kept a home server in various configurations over the last several years. Back in April, with an abundance of time at home due to the pandemic, I migrated my server from a old Dell tower running Ubuntu Server to a Raspberry Pi 4 that I had bought a while back and let sit on the shelf. That was fun to tool around with Raspbian and set it up for local network backups and media streaming. But recently it appears that the RPi SD card has gone read-only. I tried repairing it but nothing seems to work and I had thought about swapping to an alternate setup so I could experiment with the RPi for other projects anyway. So rather than flash the SD card and try to rebuild my server config (if the card isn't essentially dead), I'm going to swap my server setup to an old Mac Mini I have collecting dust. I had previously installed Ubuntu on it in order to format drives and get the RPi server setup anyway, so the big portion is already done. 

I'll need it to have **ssh** and **vnc**, as it will sit headless next to my router, as well as **sabnzbd** for newsgroups and **plexmediaserver** for local streaming. It will have two external 4TB hdd's for media storage and local backups of my other computers, and I'll also need **cron** jobs to backup the one 4TB hdd to the other, and to move downloaded .nzb files to the watched folder for plex. I think that's it? I'll edit this if I remember anything else. Super simple compared to what some other folks do with their home servers. I'd like to get more use out of it eventually, maybe consolidating music storage to stream within the house.

### HARDWARE SETUP
Mac Mini (running Ubuntu)  
4TB external drive, which Ubuntu mounts at `/media/USERNAME/storage`  
4TB external drive, which Ubuntu mounts at `/media/USERNAME/bu-storage`  

### SSH
To install **ssh**: `sudo apt install openssh-server`  
To open port 22 on the firewall: `sudo ufw allow ssh`  
You can verify the status with: `systemctl status ssh`  
and start, stop, or restart it with `systemctl start/stop/restart ssh`  
([Source](https://linuxconfig.org/ubuntu-20-04-ssh-server))

To copy a local machine's public key to the server: `ssh-copy-id -i ~/.ssh/id_rsa.pub USERNAME@servername.local`  
Test log in with: `ssh USERNAME@servername.local`  
([Source](https://linuxconfig.org/how-to-generate-and-manage-ssh-keys-on-linux))

### VNC
To install **vnc**: `sudo apt install tightvncserver`  
Configure a password: `vncpasswd`  
Install **xfce4**: `sudo apt install xfce4 xfce4-goodies`  
Create **~/.vnc/xstartup**: `nano ~/.vnc/xstartup` and enter:
```
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4 &
```
Make it executable: `chmod +x ~/.vnc/xstartup`  
Service can be started or stopped with: `sudo service vncserver@1 start/stop`  
To have **vncserver** run on startup, create:  
`sudo nano /etc/systemd/system/vncserver@.service`  
Fill it as below. Replace "USERNAME" with the local user:
```
[Unit]
Description=Systemd VNC server startup script for Ubuntu 20.04
After=syslog.target network.target

[Service]
Type=forking
User=USERNAME
ExecStartPre=-/usr/bin/vncserver -kill :%i &> /dev/null
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1920x1080 :%i
PIDFile=/home/USERNAME/.vnc/%H:%i.pid
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```
To have VNC desktop 1 start after reboot: `sudo systemctl enable vncserver@1`  
([Source](https://linuxconfig.org/vnc-server-on-ubuntu-20-04-focal-fossa-linux))

### SABNZBD
To install **sabnzbd**: `sudo apt install sabnzbdplus`  
To start or stop `sudo service sabnzbdplus start/stop`  
Create folders:  
`/media/USERNAME/storage/media/downloads/complete`  
`/media/USERNAME/storage/media/downloads/incomplete`  
`/media/USERNAME/storage/media/downloads/watched`  
Open `http://localhost:8080/sabnzbd` to enter provider info and point to **completed**, **incomplete**, and **watched** folders  
To run the service on startup, edit `/etc/default/sabnzbdplus` and set the required `USER=` to
the local account  
([Source](https://sabnzbd.org/wiki/installation/install-ubuntu-repo))  

### PLEX MEDIA SERVER

Install: `sudo apt install plexmediaserver`  
Run `sudo chmod o+rx /media/USERNAME` so the **plex** user has [necessary read and execute permissions](https://support.plex.tv/articles/201543057-why-is-some-of-my-content-not-found/) on the media libraries  
In the browser, open `http://127.0.0.1:32400/web` to configure through the GUI  
Add a Library, name it **Movies** and point it to `/media/USERNAME/storage/media/movies`  
Add a Library, name it **TV Shows** and point it to `/media/USERNAME/storage/media/tv`  
([Source](https://support.plex.tv/articles/200288586-installation/))  

### CRON

Scripts to be run with **cron** live in `/home/USERNAME/crontasks`   
Backup external storage drive with `backup_storage.sh`:  
```
#!/bin/bash  
rsync -avP --delete /media/USERNAME/storage/ /media/USERNAME/bu-storage/ 2> /home/USERNAME/crontasks/logs backup_storage.err
```
Move downloaded .nzb files to the Plex **watched** folder with `move_nzb.sh`:  
```
#!/bin/bash
mv /home/USERNAME/Downloads/*.nzb* /media/USERNAME/storage/media/downloads/watched/ > /dev/null 2>&1
```
Load these scripts into **cron** with `crontab -e`:  
```
0 5 * * * /home/USERNAME/crontasks/backup_storage.sh  
0 * * * * /home/USERNAME/crontasks/move_nzb.sh
```  
Careful with your **cron** syntax. Check out [Crontab Guru](https://crontab.guru/) to verify any new settings.