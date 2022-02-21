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