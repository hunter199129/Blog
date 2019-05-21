---
title: 'Build and Use a Simple Private Docker Registry'
date: 2019-05-17 11:53:00
tags: [docker, registry]
categories: DevOps
---

Here demostrate how to build a simple private docker registry and get image from it

Which is very useful in limited network envornment

<!--More-->

## Build registry server

    $ docker run -d -p 5000:5000 --name registry registry

Could add volumn to keep images in the host rather than registry container.

    -v /home/<user>/storage:/var/lib/registry

To enable delete images

    -e REGISTRY_STORAGE_DELETE_ENABLED=true

## Configure security setting

1. Add a new file `daemon.json` to `/etc/docker/`

        $ vim /etc/docker/daemon.json

        {
          "live-restore": true,
          "insecure-registries": ["192.168.0.1:5000"]
        }

[live-restore](https://docs.docker.com/config/containers/live-restore/) (optional): When docker daemon is down, containers will stay alive.

2. Restart docker daemon.

## Push image to registry server

    # Must tag first before push
    $ docker tag <image>[:tag] <ip>:<port>/<image>[:tag]
    
    # Push to registry
    $ docker push <ip>:<port>/<image>[:tag]

### example 

    $ docker tag hello-world:nanoserver 192.168.0.1/hello-world:my-version
    $ docker push 192.168.0.1/hello-world:my-version

## Pull image from remote

1. You will also need to set the [security setting](#configure-security-setting) which is mentioned above.

2. Pull the image from registry

        $ docker pull <ip>:<port>/<image>[:tag]

## Graphical User Interface

I use [crane](https://hub.docker.com/r/parabuzzle/craneoperator/) as GUI, because it's ease to use, no complex feature like the others.

Just easily run it as a container

    docker run -d \
      -p 80:80 \
      -e REGISTRY_HOST=registry.yourdomain.com \
      -e REGISTRY_PORT=443 \
      -e REGISTRY_PROTOCOL=https \
      -e SSL_VERIFY=false \
      -e ALLOW_REGISTRY_LOGIN=true \
      -e REGISTRY_ALLOW_DELETE=true \
      parabuzzle/craneoperator:latest

## Delete image

Use the script of [jaytaylor's gist](https://gist.github.com/jaytaylor/86d5efaddda926a25fa68c263830dac1)

    registry='localhost:5000'
    name='my-image'
    curl -v -sSL -X DELETE "http://${registry}/v2/${name}/manifests/$(
        curl -sSL -I \
            -H "Accept: application/vnd.docker.distribution.manifest.v2+json" \
            "http://${registry}/v2/${name}/manifests/$(
                curl -sSL "http://${registry}/v2/${name}/tags/list" | jq -r '.tags[0]'
            )" \
        | awk '$1 == "Docker-Content-Digest:" { print $2 }' \
        | tr -d $'\r' \
    )"
    
Garbage cleanup after deleted the image
 
    docker exec -it docker-registry bin/registry garbage-collect /etc/docker/registry/config.yml
