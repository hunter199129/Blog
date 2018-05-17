---
title: '[Windows] 右鍵選單加入Powershell'
date: 2017-10-06 16:00:00
tags: [Windows, Regedit, Powershell, cmd]
categories: Windows
---

Windows的cmd一直都比Linux的難用很多，用起來就是一整個不習慣，最麻煩的就是要找路徑，雖然複製路徑很方便了，但是如果能直接右鍵打開不是更好？所以就找了方法去做這件事。

<!--More-->

## 用Regedit編輯機碼

用windows+R打開執行，開regedit，然後找到`HKEY_CLASSES_ROOT\Directory\Background\shell`

在shell點右鍵 > 新增機碼

打 **Powershell**

預設值打 **&Powershell** (這邊是右鍵選單顯示名稱)

再在Powershell資料夾新增機碼 **command**

預設值: **C:\WINDOWS\system32\WindowsPowerShell\v1.0\Powershell.exe** (Powershell的路徑)

接下來右鍵就會有Powershell的選項了！

## 寫登錄檔

這樣自己用還可以，可是如果要教別人還是有點麻煩，而且要找機碼也不方便，所以乾脆直接寫一個reg檔更快。

	Windows Registry Editor Version 5.00

	[HKEY_CLASSES_ROOT\Directory\Background\shell\Powershell]
	@="&Powershell"
	
	[HKEY_CLASSES_ROOT\Directory\Background\shell\Powershell\command]
	@="C:\\WINDOWS\\system32\\WindowsPowerShell\\v1.0\\Powershell.exe"

把這段直接存成reg檔，接下來只要執行就會把Powershell加進去囉，這樣應該是最懶人的辦法了。

## 後記

當然這個方法不只有Powershell能用，任何自己的程式都可以加，而且這樣可以直接找到當前的路徑，真得很方便。

不過這只有針對資料夾的部分，針對檔案的部分之後再補上。

這裡有可以參考的網站先看看:

<https://www.howtogeek.com/107965/how-to-add-any-application-shortcut-to-windows-explorers-context-menu/>
