---
title: "Home Server"
date: 2020-12-16T11:11:57-05:00
draft: true
author: Mike Quigley
---

I've kept a home server in various configurations over the last several years. Back in April, with an abundance of time at home due to the pandemic, I migrated my server from a old Dell tower running Ubuntu Server to a Raspberry Pi 4 that I had bought a while back and let sit on the shelf. That was fun to tool around with Raspbian and set it up for local network backups and media streaming. But recently it appears that the RPi SD card has gone read-only. I tried repairing it but nothing seems to work and I had thought about swapping to an alternate setup so I could experiment with the RPi for other projects anyway. So rather than flash the SD card and try to rebuild my server config (if the card isn't essentially dead), I'm going to swap my server setup to an old Mac Mini I have collecting dust. I had previously installed Ubuntu on it in order to format drives and get the RPi server setup anyway, so the big portion is already done. 

I'll need it to have **ssh**, as it will sit headless next to my router, as well as **sabnzbd** for newsgroups and **plexmediaserver** for local streaming. It will have two external 4TB hdd's hanging off it for media storage and local backups of my other computers, so I'll need to setup **fstab** to allow those to mount on boot and I'll also need **cron** jobs to backup the one 4TB hdd to the other, and to move downloaded **.nzb** files to the watched folder for plex. I think that's it? I'll edit this if I remember anything else. Super simple compared to what some other folks do with their home servers. I'd like to get more use out of it eventually, maybe consolidating music storage to stream within the house.

### SSH
To install **ssh**: `sudo apt install openssh-server`  
To open port 22 on the firewall: `sudo ufw allow ssh`  
You can verify the status with: `systemctl status ssh`  
and start, stop, or restart it with `systemctl start/stop/restart ssh`  
([Source](https://linuxconfig.org/ubuntu-20-04-ssh-server))

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
Fill it as below. Replace "user1" with the local user:
```
[Unit]
Description=Systemd VNC server startup script for Ubuntu 20.04
After=syslog.target network.target

[Service]
Type=forking
User=user1
ExecStartPre=-/usr/bin/vncserver -kill :%i &> /dev/null
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1920x1080 :%i
PIDFile=/home/user1/.vnc/%H:%i.pid
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```
To have VNC desktop 1 start after reboot: `sudo systemctl enable vncserver@1`  
([Source](https://linuxconfig.org/vnc-server-on-ubuntu-20-04-focal-fossa-linux))

### SABNZBD
To install **sabnzbd**: `sudo apt install sabnzbdplus`  
To start or stop `sudo service sabnzbdplus start/stop`  
Open `http://localhost:8080/sabnzbd` to setup and enter provider info  
To run the service on startup, edit `/etc/default/sabnzbdplus` and set the required `USER=` to
the local account  
([Source](https://sabnzbd.org/wiki/installation/install-ubuntu-repo))
* To Do:  
  * Create "watched" folder in /mnt/storage/media/downloads  
  * Move "complete" & "incomplete" folders from ~/Downloads to /mnt/storage/media/downloads

### PLEXMEDIASERVER

### FSTAB

### CRON

### Additional tools
tldr


