---
title: 'Make jupyter-lab ip accessable under CentOS 7'
date: 2018-09-19 14:15:00
tags: [anaconda, jupyter, memo]
categories: Memo
---

Enabling client accessability for jupyter-lab

<!--More-->

Modify the jupyter config

    $ vim ~/.jupyter/jupyter_notebook_config.py

then add 

    c.NotebookApp.ip = '*'
