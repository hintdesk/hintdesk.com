---
title: "Some useful linux and docker commands"
date: 2022-02-21 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: docker
tags: docker
---

## Intro
In this post I would like to list some useful commands for using Docker in Linux.

## Commands

- List all images with their IPs.

```terminal
docker ps -q | xargs -n 1 docker inspect --format '{{ .Name }} {{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}}' | sed 's#^/##';
```

- Delete all obsolete images.

```terminal
docker system prune --volumes
```

- List folder with size.

```terminal
du -h --max-depth=1
```

- Start a dummy image to create a network interface with specific IP sub-range.

```terminal
version: '3.5'

services:
  network_default:
    image: hello-world:latest
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      config:
      - subnet:  192.168.0.0/16
```
