---
title: 'How to modify and commit changes to a docker image'
date: 2018-07-27 9:30:00
tags: [docker]
categories: DevOps
---
Customize image is the must if we're using other's docker image.

Something like install new packages, drivers, plugins of application, or setting global variables, etc.

<!--more-->

so guide below is how you manipulate the docker image

## Run image file as a container

    sudo docker run --name CONTAINER_NAME -p PORT:PORT -e TERM=xterm -d <DOCKER_IMAGE>

## Use command in container environment

    sudo docker exec -it <CONTAINER_ID> bash

## Install packages you need

    apt install <package>
    pip install <python-package>

## Commit change and save to new image

    sudo docker commit <CONTAINER_ID> superset-mssql

## Check image

    sudo docker images

