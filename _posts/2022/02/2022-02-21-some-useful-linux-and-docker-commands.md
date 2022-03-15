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

## Dockers

- List all images with their IPs.

```terminal
{% raw %}docker ps -q | xargs -n 1 docker inspect --format '{{ .Name }} {{range .NetworkSettings.Networks}} {{.IPAddress}}{{end}}' | sed 's#^/##';{% endraw %}
```

- Delete all obsolete images, volumes and networks...

```terminal
docker system prune --volumes
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

and then use the network for the image as following

```terminal
networks:
  default: 
    external:
      name: network_default
```

## Linux

- List all folders of the first level with size.

```terminal
du -h --max-depth=1
```

- Check if port is in use

```terminal
sudo lsof -i -P -n
```

- Update Ubuntu system

```terminal
sudo apt-get update      
sudo apt-get upgrade      
sudo apt-get dist-upgrade  
sudo apt-get autoremove
sudo reboot
```

- Show summary info of Ubuntu system

```terminal
landscape-sysinfo
```

- To install .Net Core on Linux Arm64

https://docs.microsoft.com/en-us/dotnet/core/install/linux-scripted-manual#manual-install

Use this command to reload bash and make **dotnet** command available.

```terminal
source ~/.bashrc
```

- Enable **root** for SSH.

```terminal
sudo passwd root
sudo vi /etc/ssh/sshd_config
```

Set following properties

```terminal
PermitRootLogin yes
PasswordAuthentication yes
```

Restart **SSH**

```terminal
sudo service sshd restart
```

- Install Certbot and create certificate for a domain

```terminal
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d {domain}
```
