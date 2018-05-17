---
title: '防止vim產生un~檔案'
date: 2017-03-23 19:41:00
tags: [vim, ide, vimrc]
categories: Linux
---

vim本身預設會在自己的資料夾下建立`.~`和`.un~`的檔案，目的是用來做更改紀錄和備份。  
但有時候就覺得這檔案有點煩，要複製資料夾的時候會複製到也很不方便，所以就要去`vimrc`改config把他改掉
	
<!--More-->

## vimrc設定
    
	set noundofile
    set nobackup
    set noswapfile

這三行就是設定把他全部關掉，如果想保留到其他資料夾下，可以用

	undodir=~/.undodir

其他其實在設定文件裡也寫得很明顯，就花點時間看看

## buftype設定

另外存檔如果遇到報錯*E382: Cannot write, 'buftype' option is set*

可以在vim裡面下`:verbose set buftype`去看，當buftype=nofile的時候不能存檔；當buftype=*null*的時候才可以存檔

可以用`:setlocal buftype=`改buftype就可以存檔了

如果要回復就下`:setlocal buftype=nofile`
