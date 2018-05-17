---
title: Hexo theme settings
date: 2018-05-17 11:06:46
tags: [hexo, blairos, hexo theme]
categories: Blog
---

好久沒有來寫blog，看到之前灌的theme裡面code block的部分真是越看越怪

想說最近應該有新的theme可以用了，就來順手換一下

<!--More-->

最後找到一個不錯的theme: [blairos](https://github.com/52binge/hexo-theme-blairos)

看起來很簡單也很順眼，就試著設定一下，這邊就記一下設定hexo一些眉眉角角的地方

## Favicon

在theme的`_config.yml`最下面就有favicon可以設定

但是不知道為什麼，檔案路徑怎麼抓就是抓不到

最後直接丟上傳到github的url給他就好了

## Categories

### Categories page & Tags page

我的blog本來是沒有categories的，所以現在要把他加上去

1. 要先建出一個categories的page


    hexo new page categories

2. 把剛剛建的page類型設為categories


    ---
    title: categories
    date: 2018-05-17 10:47:04
    type: "categories"
    ---

  *Note:* Discus評論記得要關掉
    
    ---
    title: categories
    date: 2018-05-17 10:47:04
    type: "categories"
    comments: false
    ---

3. 這時候雖然會有分類，但是categories page應該是空的  
   所以要再加上 `layout: categories` 讓這個page套用theme的規則


    ---
    title: categories
    date: 2018-05-17 10:47:04
    layout: categories
    type: "categories"
    comments: false
    ---

tag也是一樣的步驟，只是把**categories**都換成**tags**

### Post

文章的部分就是在標頭加上tags跟categories


    ---
    title: Hexo theme settings
    date: 2018-05-17 11:06:46
    tags: [hexo, blairos, hexo theme]
    categories: Blog
    ---


## Table of Content

這個主題預設就有toc(Table of Content)了

不過預設會有標數字，要把自動標數字關掉要找到 `article.ejs`

把裡面的 `<%- toc(post.content) %>` 改成 `<%- toc(post.content, {list_number:false}) %>` 即可

## Read more

這個就比較簡單，只要在文章要顯示read more的地方加上 `<!--More-->` 即可