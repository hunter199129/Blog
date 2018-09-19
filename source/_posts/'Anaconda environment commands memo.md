---
title: 'Anaconda environment commands memo'
date: 2018-09-19 14:00:00
tags: [python, anaconda, memo]
categories: Memo
---

This is a memo for anaconda env setting commands

<!--More-->

## Create env offline

Installed packages python, pip, pandas

    $ conda create --no-default-packages -n myEnv --offline python pip pandas

NOTE: Install pip to split base env pip, which means pip install packages will only available for new env instead of global

## Activate/Deactivate env 

### Windows:

    $ activate myEnv

    $ deactivate

### Linux: 

    $ source activate myEnv

    $ source deactivate

## Remove env

    $ conda remove --name myEnv --all
