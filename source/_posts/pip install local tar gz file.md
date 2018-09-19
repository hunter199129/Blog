---
title: 'pip install local tar gz file'
date: 2018-09-19 14:00:00
tags: [python, pip, memo]
categories: Memo
---

Pip install local tar.gz package may fail sometimes, and then it'll try to get the package online, but if there's no network connection, it'll fail

<!--More-->

So, to install tar.gz file

First unzip the package then cd to the folder, do

    $ python setup.py build
    $ python setup.py install
