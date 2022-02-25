---
title: "How to start an Elasticsearch Cluster by Docker compose?"
date: 2022-02-24 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: elasticsearch
tags: elasticsearch
---

## Intro
In this post I would like to document how I could start an Elasticsearch Cluster by Docker compose and how I resolve the out of memory error during installation. For detailed instructions and explainations, you can follow this link [elastic/get-started](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-stack-docker.html). 

## Steps

- Increase the **max_map_count** to solve the error

```terminal
Max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

or 
```terminal
There is insufficient memory for the Java Runtime Environment to continue.
Native memory allocation (mmap) failed to map 65536 bytes for committing reserved memory.
An error report file with more information is saved as
```

by executing following commands

```terminal
sudo vi /etc/sysctl.conf
```

Add or edit **vm.max_map_count**

```terminal
vm.max_map_count=262144
```

- Go to [elastic/stack-docs](https://github.com/elastic/stack-docs/tree/main/docs/en/getting-started/docker), download **.env** and **docker-compose.yml** files.

- Edit **.env** file with the your settings. 
- Add this option below to **.env** for setting Java Heap Size.

```terminal
ES_JAVA_OPTS=-Xmx256m -Xms256m
```

- Edit **docker-compose.yml** with your settings. 
- Use the option **ES_JAVA_OPTS** in section **environment** of **docker-compose.yml** file to use Java Heap Size setting in Docker images. This option should be added for all nodes (es01,es02,es03).

```terminal
environment:
  - ES_JAVA_OPTS=${ES_JAVA_OPTS}
```

- Then now you can start Elasticsearch Cluster with 

```terminal
docker-compose up -d
```