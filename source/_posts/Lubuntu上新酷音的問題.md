title: Lubuntu上新酷音的問題
date: 2017-03-09 13:39:00
tags: [Ubuntu, 輸入法, 新酷音]
---
因為家裡的這台筆電比較舊了，所以就想說換一個輕一點的系統來用用。

剛開始本來是安裝elementary os上來的，但是因為eOS上的graphic driver好像不太支援，加上這台(inspiron-14-3452)是用Intel的內顯，偏偏Intel又沒有支援eOS的安裝程式，所以就直接改用Lubuntu。

遇到的問題是新酷音的輸入法會有一個黑框在底下，所以就去找了解法：

> 從偏好設定 > LXSession的預設應用程式

> 點選自動啟動把FcitxQt IMPanel啟用取消勾選

> 最後重新登入

這樣就解決了，順手紀錄一下，因為之後非常有可能還會用到！
