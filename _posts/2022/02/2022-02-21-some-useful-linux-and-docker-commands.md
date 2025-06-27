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

- List all files of a volume

```terminal
docker run -it --rm -v named_volume:/vol busybox ls -l /vol
```

- Delete all obsolete images, volumes and networks...

```terminal
docker image prune -a
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

- Save image to file.

```terminal
docker save -o ./{filename}.tar {image_name}
```

The *image_name* can be get through

```terminal
docker images
```

- To install *.tar* file

```terminal
docker load -i {filename}.tar
```


## Linux

- List all folders of the first level with size.

```terminal
du -h --max-depth=1
```

or 

```terminal
sudo df -h
```

- Check if port is in use

```terminal
sudo lsof -i -P -n
```

or 

```terminal
sudo lsof -i -P -n | grep dotnet
```


- List all opened ports through firewall

```terminal
sudo firewall-cmd --list-all
```

- Update Ubuntu system

```terminal
sudo apt-get update      
sudo apt-get upgrade      
sudo apt-get dist-upgrade  
sudo apt-get autoremove
sudo reboot
```

- Show summary info of Ubuntu system.

```terminal
landscape-sysinfo
```

- Enable **root** for SSH.

```terminal
sudo passwd root
sudo vi /etc/ssh/sshd_config
```

Set following properties.

```terminal
PermitRootLogin yes
PasswordAuthentication yes
```

Restart **SSH**.

```terminal
sudo service sshd restart
```

- Install Certbot and create certificate for a domain.

```terminal
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d {domain}
```

- Check if port is accessible from outside.

```terminal
nc -vz {IP_OR_COMPUTER_NAME} 24224
```

- Create *.pem* from *.pfx* certificate

```terminal
openssl pkcs12 -in file.pfx -out file.pem -nodes
```

- Find where the environment variable is set

```terminal
grep -r VARIABLE_NAME /etc/*
```

- Extract private key from pfx and remove passphrase using OpenSSL (Use GitBash)

```terminal
winpty openssl pkcs12 -in 1.pfx -clcerts -nokeys -out 1.cer
winpty openssl pkcs12 -in 1.pfx -nocerts -out 1.private.key
winpty openssl rsa -in 1.private.key -out 1.key
```



