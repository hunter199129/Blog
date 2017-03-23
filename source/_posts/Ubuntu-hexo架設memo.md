---
title: Ubuntu hexo架設memo
date: 2016-10-06 15:43:05
tags: [hexo, blog, Ubuntu]
---
架設的途中有遇到一些小問題，但因為問題都不大，所以很快就能解決了。

還有一部份是theme的問題，想說晚點再調好了，畢竟主要是用來寫東西的，那些比較雜的東西有時間再玩。

#### 遭遇到的問題
1. Ubtunu上nodejs套件npm跟hexo對不上，解決的辦法就是讓node更新，下cli指令也可以，也不清楚我怎麼弄的，總之試了幾次就架好了。
2. hexo new "post"的時候遇到權限問題。就算用sudo去new，雖然它說有建立檔案，但卻也看不到建立的檔案。解決辦法就是直接把hexo原本的資料夾砍掉，重新hexo init一個新的資料夾就好了。

#### Reference Sites
- [手把手教你使用Hexo + Github Pages搭建个人独立博客](http://div.io/topic/1691)
- [Hexo Doc](https://hexo.io/zh-tw/docs/)
- [git push用法和常见问题分析 ](http://www.cnblogs.com/renkangke/archive/2013/05/31/conquerAndroid.html)
- [Hexo 安裝教學、心得筆記 ](https://wwssllabcd.github.io/blog/2014/12/22/how-to-install-hexo/)
- [使用 Hexo 與 Github 架設自己的 Blog](http://blog.thepetertung.com/2016/07/28/how-to-use-hexo-to-build-your-blog-1/)
