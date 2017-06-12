---
title: 'Install Wireless Network Driver Using Ubuntu live usb'
date: 2017-06-12 20:20:00
tags: [Ubuntu, Installation, Wifi]
---
經過上次心血來潮更新Ubuntu到17.10之後，遇到的問題只有越來越多。

從剛開始開機會一直跳錯誤，到後來Software Center幾乎不能用，到最後每次開機都要去`/etc/resolv.conf`重設定一次DNS server，不然不能上網，最後只好認命重灌。

中間也試了一下用看看Arch-base的[Manjaro](https://manjaro.github.io/)，雖然也不算難用，但就是不習慣。

然後灌了Ubuntu 16.04回來，發現竟然不能上網，找到問題點就是在沒有無線網卡驅動。

既然知道問題就不用多說，Google餵下去看看有沒有解答先。

找到的解法就是這兩行Bash script:

    sudo dpkg -i /media/username/volname/pool/main/d/dkms/dkms_*.deb
    sudo dpkg -i /media/username/volname/pool/restricted/b/bcmwl/bcmwl-kernel-source_*.deb
    
其中*username*跟*volname*是變數，反正就是從live usb裡面找到這兩個檔案安裝就對了。

然後就會發現有無線網路可以用了！

P.S.灌了16.04果然穩定很多，以後還是不要亂裝更新好了，畢竟我也不懂Linux開發...

不過有興趣的話倒是滿值得玩玩學學的。

---
Ref:
[askubnutu - how can i install and download drivers without internet](https://askubuntu.com/questions/146425/how-can-i-install-and-download-drivers-without-internet)
