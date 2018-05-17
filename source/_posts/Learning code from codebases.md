---
title: 'Learning code from codebases'
date: 2017-10-03 13:11:00
tags: [Learning, Code Skill, Github]
categories: Personal
---

今天早上看到了一篇滿有趣的文章: [Interest Codebases](https://medium.com/@markpapadakis/interesting-codebases-159fec5a8cc)。

內容分為兩個部分。  

<!--More-->

第一部分是作者在講述他喜歡從別人的code裡面看一些好的Code design, algorithms, tricks等等。  
第二部分就是列出一些他覺得不錯的Codebases，並寫一些簡短的原因。  
不過其實內容講述的有點少，剛好他有附上[HackerNews](https://news.ycombinator.com/item?id=15371597)上的連結，就也進去看了一下。

HackerNews裡面的討論就稍微有些內容了，有些人會提出一些問題或是想法，有些會推薦哪些是適合入門或是很有架構的，下面列出一些我覺得比較有討論價值的

+ 我讀codebases遇到最大的問題是，如果我不熟悉全部的語言或是某個語言的架構，我該從哪裡開始？最終我只會浪費很多時間讀那些可能不是主要運作的程式。....
	+ 作者回應: 通常我會有特定想要了解的部分，像是以[Seastar](https://github.com/scylladb/seastar)為例，我想了解他們的reactor設計和實作，所以我會找到他們實作這個部分的地方，然後從這開始，再展開到其他的檔案。--通常或會開很多vim視窗，然後做筆記。  
如果沒有特定想了解的部分，我會選一個資料夾，然後依檔案大小排序(e.g ls -lShr *.{h,cpp,hh,cc,java})。我通常從小到大排序，可是有時候從大到小開始看會更合理，我就從這開始。然後我如果找到什麼其他的東西我就會開新視窗去看，然後再回到原來的檔案。  
		+ 或是看看最多更動的檔案  
	+ 如果有問題，`grep -r -- 'main[ ]*('`  
	+ 最先要做的事，clone and build。如果你沒辦法clone, build，然後在一小時內弄好它，基本上這東西就是垃圾。(開發者應該要讓他們的projects盡量簡單)。build完之後就可以做一些小改動去驗證你的理解。

+ 推薦書籍: [The Architecture of Open Source Applications](http://aosabook.org/en/index.html)，每個章節都是不同open-source project的歷史敘述跟架構。

+ [Redis](https://github.com/antirez/redis)是讓我印象最深刻的code base。簡單來說，有可讀的函式跟註解使它很容易學習。

也有些人質疑他根本不了解全部的code，他的回應是說沒錯，但也不需要全部了解，只需要了解有興趣的內容。我覺得code本質也是這樣，如果全部都要學全絕對是不可能的事，而且資訊的東西進展得這麼快，針對某個有興趣的領域去讀是一定的，不然包山包海，能針對特定領域了解透徹就已經是很了不起的事了!

整篇讀下來最多被Reference到的就是[Github](https://github.com/)了。由此可見Github對於開發者的重要性，沒事的時候去Github挖挖寶，看看別人的架構(尤其是很多大公司像[Google](https://github.com/google), [facebook](https://github.com/facebook))的code，對學習一定是很有幫助的!
