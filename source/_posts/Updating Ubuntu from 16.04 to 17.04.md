---
title: 'Updating Ubuntu from 16.04 to 17.04'
date: 2017-05-03 22:00:00
tags: [Ubuntu]
categories: Linux
---

突然不知道從哪看到ubuntu 17.04已經出了，就心血來潮想說來灌一下，結果安裝還花了我不少時間。

中間還遇到一點小問題，不過還好只是小問題，不然我電腦要直接重灌了…

<!--More-->

## 16.04 -> 16.10 -> 17.04

先更新目前的軟體，確保所有軟體都在最新狀態

    sudo apt update && sudo apt dist-upgrade
  
再來要先去

  > **軟體和更新** > 更新 > 通知我新的Ubuntu版本 選任何新版本
  
![Update1](http://i.imgur.com/dZe6Rnk.png)

然後有兩種方式可以安裝

圖形化界面

    sudo update-manager -d
  
文字界面
 
    sudo do-release-upgrade -d
  
安裝約要花2~3小時（也可能是因為我的網速慢所以比較久，不過1小時跑不掉）

16.10 -> 17.04 也是一樣的指令

不過在升到17.04之前，要先把第三方軟體都關掉

![Update2](http://i.imgur.com/9HY1C91.png)

然後我在這有遇到一個問題，它不讓我更新，我還去回報了bug。

結果最後找到原因是有壞掉的package還在，所以不能更新，所以這時候就要把壞掉的package給砍掉

要先找出壞掉的package

    grep Broken /var/log/dist-upgrade/apt.log
    
列出來之後就用`sudo apt autoremove`慢慢砍吧…
    
然後再下安裝指令就可以裝了！

## 查看版本

查看版本的指令是

    lsb_release -a
    
這是linux內建的，不過我喜歡另一個

安裝**neofetch**

    sudo apt install neofetch

再執行`neofetch`

why? 因為這個比較fency阿! XD

## 裝好之後，Desktop壞了！？

既然很明顯是Desktop壞了，就直接對症下藥吧

直接下指令把他裝回去

    sudo apt-get install ubuntu-desktop
    
解決

聽說從17.10還是18.04之後，unity就會被廢了，就會改成用**gnome**

所以本來是想說可以直接上gnome，但是硬裝之後沒辦法用

所以就算了，這次就先弄到這好了

這樣裝下來大部分的東西都還在，其實還滿方便的，希望之後不要遇到什麼大bug就好。

## Ref:

- [2 Ways to Upgrade From Ubuntu 16.10 to Ubuntu 17.04 (Graphical & Terminal)](https://www.linuxbabe.com/ubuntu/upgrade-ubuntu-16-10-to-17-04)

- [How to Upgrade to Ubuntu 17.04 from Ubuntu 16.10](http://ubuntuhandbook.org/index.php/2017/04/upgrade-to-ubuntu-17-04-from-ubuntu-16-10/)

- [askubuntu - Could not calculate the upgrade, what happened?](https://askubuntu.com/questions/360293/could-not-calculate-the-upgrade-what-happened)

- [ubuntu forum - How to fix Broken Desktop](https://ubuntuforums.org/showthread.php?t=1895565dd)
