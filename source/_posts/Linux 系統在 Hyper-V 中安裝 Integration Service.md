---
title: 'Linux 系統在 Hyper-V 中安裝 Integration Service'
date: 2018-06-29 15:36:00
tags: [Hyper-V, Linux]
categories: Devops
---
在使用 Hyper-V 時，如果 Linux 沒有安裝 Integration Service 會有很大的問題

像是: 在桌面環境不能使用滑鼠、網路找不到 MAC 等等

<!--More-->

安裝的方法其實很簡單，以 CentOS 為例

1. 去 Microsoft 官網下載 [Linux Integration Disk](https://www.microsoft.com/en-us/download/details.aspx?id=55106)
2. 把 ISO 掛載到 Instance 上
3. 想辦法打開 Terminal (CentOS 可以用 **Alt + F1** 開啟選單)
4. `cd /media/CDROM`
5. 找到 Linux 的版本，執行相對應資料夾下的 `install.sh` (CentOS 6.3 版就是找 **RHEL63** 的資料夾)
6. 重開機之後，就會發現一切正常了!
