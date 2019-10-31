---
title: '在內網環境架設 Docker Redmine'
date: 2019-10-31 08:33:00
tags: [redmine, docker, linux]
categories: DevOps
---

Docker 在內網環境真的很難弄，只能 pull images 其他的軟體或套件都要自己拉下來處理...

部門之前架了 Redmine 覺得很好用，現在因為某些需求須要更新版本，所以之前的東西都要全部重 build 一次

之前的做法太不方便了，所以這次想了新的方法 -> 改 Docker image

<!--More-->

## Base Redmine Image

這邊用的是 [sameersbn/docker-redmine](https://github.com/sameersbn/docker-redmine)  
一方面是之前就是用他的 Image，另外就是之前用過就他這個版本最方便  
所以就用他的版本來改成我們需要的  
就先 fork 一份到自己的 Github 上

## Plugins

要弄這個最麻煩的就是要裝套件，主要就是因為有些套件在裝的時候要去外面抓 Ruby 的 gem，也就是 library  
所以都要先裝好在 image 裡面才行  

### Gemfile
在 Repo 裡加上一個 Gemfile，讓 Ruby 知道要去抓哪些 gems，那我們又怎麼知道要抓哪些 gems?  
就要去每個要裝的套件裡面，找出 `Gemfile` (或是有些會寫 `Gemfile_for_test`)，把它整合到一份 `Gemfile` 裡面

在 `Dockerfile` 的部分就要加上設定安裝這些套件，用的是 Ruby 的套件管理軟體 [Bundler](http://bundler.io)  
原本的 image 已經幫我們裝好了 `gem install --no-document bundler`

```Dockerfile
COPY Gemfile .
RUN bundle config --global silence_root_warning 1 \
&& bundle install
```

第二行的作用是 bundler 用 root 安裝會有警告，會導致 build image 失敗

### Software
有些 gems 在安裝的時候會有相依軟體的問題，像是 `pg` 會需要裝 postgres 在 linux 裡面才可以  
這些就在 `Dockerfile` 把這些需要的軟體先裝上去

```Dockerfile
RUN apt update \
&& apt -y install libmysqlclient-dev libpq-dev libmagick++-dev \
&& apt -y install software-properties-common \
&& add-apt-repository ppa:libreoffice/ppa \
&& apt update  | grep packages \
&& apt -y install libreoffice --no-install-recommends \
&& apt -y install unzip
```
## Build

```shell
docker build -t="test/redmine" .
```

## Note
最後這些都灌進去了也不能先把套件裝在 plugins 的資料夾下  
因為這個 Image 要裝到 `/home/redmine/data/plugins` 才會安裝套件，如果先直接丟到裡面套件會讀取不到  
所以要等開過 Redmine 一次之後(資料夾被建出來)，才可以裝套件進去  
因為需要用到的 gems 都放進去了，所以套件裝進去就可以直接使用了

附上 Repo: https://github.com/hunter199129/docker-redmine
