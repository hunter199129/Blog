---
title: Windows host share folder to VMPlayer CentOS 7
date: 2018-09-19 16:02:00
tags: [CentOS, Windows, vmware, memo]
categories: Memo
---

Setting share folder using VMware player

<!--More-->

## VM Settings

Under Player > Manage > VM Settings > Options page > Share Folders property

select folder to share

## Install VMware tools

### Insert cd

Player > Manage > Install/Reinstall VMware tools

### Mount cd

    $ sudo mkdir /mnt/cdrom
    $ sudo mount /dev/cdrom /mnt/cdrom

    $ ls /mnt/cdrom
    $ tar xzvf /mnt/cdrom/VMwareTools-<version>.tar.gz -C /tmp/
    $ cd /tmp/vmware-tools-distrib/
    $ sudo ./vmware-install.pl -d

## Run config

    $ vmware-config-tools.pl

I use default setting here

## Check share folder

Share folder should under `/mnt/hgfs` folder now
