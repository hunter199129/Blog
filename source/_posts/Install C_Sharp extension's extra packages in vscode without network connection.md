---
title: 'Install C_Sharp extension extra packages in vscode without network connection'
date: 2017-11-10 15:29:00
tags: [vscode]
categories: Windows
---

安裝完C#擴充套件之後，會有兩個檔案需要再另外下載

如果vscode被防火牆擋住，又沒辦法設定proxy，每次重開vscode的時候都會去重載那兩個檔案

<!--More-->

然後一直失敗

解決的辦法是

### 1. 先把兩個檔案都在載下來

在output或是package.json裡面都可以找到載點

### 2. 把檔案放到相對應的資料夾下

vscode在windows底下擴充套件的資料夾位置是`%userprofile%\.vscode\extensions`

再來找到`ms-vscode.csharp-*`的資料夾

Omnisharp的zip解壓縮之後放到`bin\omnisharp`

debugger放到`.debugger`，這邊新增資料夾的時候，用GUI新增會出問題

所以開cmd下`mkdir .debugger`就可以建了

### 3. 最後新增一個空的`install.Lock`檔

這樣就完成了

如果沒有新增install.Lock檔，系統還是會自己去下載檔案
