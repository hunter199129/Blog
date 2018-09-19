---
title: 'Using Docker in off-line environment on CentOS 7'
date: 2018-07-26 15:14:00
tags: [CentOS, docker]
categories: DevOps
---

Sometimes we need to build server at the internal network environment.

So, here's the instruction how to build docker image to container in off-line environment.

<!--More-->

1. Follow the [offical guide](https://docs.docker.com/install/linux/docker-ce/binaries/#prerequisites)
2. Setup docker on google cloud engine instance, which can access docker hub
3. Download image, for instance, hello-world
4. 
        sudo docker pull hello-world

5. Save docker image as file

        sudo docker save -o hello-world.docker hello-world

6. Transfer file to target server
7. Load file from target server

        sudo docker load -i hello-world.docker

