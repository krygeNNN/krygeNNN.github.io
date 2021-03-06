---
title: Rclone Systemd Startup Service
author: krygennn
date: 2021-10-9 18:17
categories: [Blogging,linux]
tags: [rclone,rsync,systemd]
---
Create a systemd file by running;
`vim /etc/systemd/system/rclone.service`
<br>Insert the following.
#### rclone.service
```
[Unit]
Description=Google Drive (rclone)
AssertPathIsDirectory=/mnt/google-drive
After=plexdrive.service

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount \
        --config=/home/{user}/.config/rclone/rclone.conf \
        --user-agent mydrive \
        --vfs-cache-mode full \
        --allow-other \
        --fast-list \
        --max-backlog 999999 \
        --drive-chunk-size 64M \
        --bwlimit-file 100m \
        --checkers=16 \
        gdrive_api: /mnt/google-drive
ExecStop=/bin/fusermount -u /mnt/google-drive
Restart=always
RestartSec=10

[Install]
WantedBy=default.target
```
Change the **user** field in **--config** with your personal user name.

Finally run `systemd enable rclone.service` and `systemd start rclone.service`

