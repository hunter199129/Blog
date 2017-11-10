---
title: '解決npm "...error errno -4048..."的問題'
date: 2017-03-23 19:41:00
tags: [npm, nodejs]
---

大略的error code如下:

    ...
    9453 error code EPERM
    29454 error errno -4048
    29455 error syscall scandir
    29456 error Error: EPERM: operation not permitted, scandir 'C:\Users\10512157\Desktop\test-one\node_modules\fsevents\node_modules'
    ...
    29457 error Please try running this command again as root/Administrator.

這個問題基本上會遇到的都是windows的使用者，不過說實在我也不太知道是什麼問題

但是這是新版的npm才會遇到的問題，看起來應該是個在windows上的bug

所以要解決這個問題，只要把npm降版就好了

npm降版的方式很簡單，直接對npm下`npm i -g npm@<version>`

這邊用win7只要降到`5.0.3`就可以用，win10我就不知道了

看起來不是什麼大問題，但也花了我不少時間研究...
