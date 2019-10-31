---
title: 'Set docker daemon as service in CentOS7'
date: 2019-10-31 14:26:00
tags: [docker, CentOS]
categories: DevOps
---

Note: You must run as `root`

<!--More-->

```shell
cd /etc/systemd/system
wget https://raw.githubusercontent.com/moby/moby/master/contrib/init/systemd/docker.service
wget https://raw.githubusercontent.com/moby/moby/master/contrib/init/systemd/docker.socket

# check status
sudo systemctl status docker
# auto start when boots
sudo systemctl enable docker
# manually start
sudo systemctl start docker
```

https://docs.docker.com/config/daemon/systemd/#manually-create-the-systemd-unit-files
