---
title: 使用Travis CI隨時隨地發Blog文章(Hexo to Github)概念篇
date: 2017-03-13 13:39:00
tags: [hexo, Travis CI, GitHub]
---
之前架好Hexo之後，沒事就可以很開心的用我的筆電無聊寫寫一些blog文，`hexo new`建template、寫好之後`hexo g -d`建立發佈，輕鬆方便。

但是之後發現這會有一個問題，就是當筆電不在我身邊的時候我就沒辦法發佈文章了。除非我有我hexo框架和文章的原始碼，就算在家裡另一台電腦我也沒辦法做。所以就誕生了這篇文章。

## Problem Scope

要解決的問題主要就是能夠在其他環境下發文。

## 最基本的想法

要達成這個目的最簡單的想法就是把hexo的原始碼push到github上的其他branch或repo，當每次要發佈文章的時候就下`git pull`，然後再做hexo的動作，最後再`git push`回去。

當然這是個好方法，而且也可以明確的解決問題，但是會有其他的需求產生。

1. 如果少做到`git pull`或`git push`，那文章的同步性就會出問題，在下次寫文章的時候會有代價。
2. 我必須要在有 *git* 的環境下才能發佈。

## 導入Travis CI

絕對不是只有我有這樣的需求，所以我就找了一下相關的解法，本來也做好要換別的blog的打算了，但最後找到了一個很好的方法可以很好的解決以上的需求，就決定來測試看看。

[Travis CI](https://travis-ci.org/)是一個持續整合的服務，一個測試code的服務。簡單來說就是會隨你的git push去抓你的code，然後跑寫好的script作測試。

所以以這個需求來說，就是讓測試跑`hexo g -d`這一行script，這樣一來就可以完成上述的需求。

那大部分的概念想法就是這樣，下一篇再詳述實作的部分。

P.S. 配合上Github可以Web就可以做`git push`，直接在Web甚至手機就可以發blog啦！
