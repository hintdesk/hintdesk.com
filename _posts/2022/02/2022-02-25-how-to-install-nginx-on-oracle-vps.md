---
title: "How to install Nginx on Oracle VPS?"
date: 2022-02-25 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: nginx
tags: nginx oracle
---

## Intro
In this post I would like to document how I install nginx and open the ports on an Oracle VPS. If you're interested in creating a free Oracle VPS, you can follow the instructions in this post [How to create a free Oracle VPS with Python script (Out of capacity)?](https://www.hintdesk.com/2022/01/15/how-to-create-a-free-oracle-vps-with-python-script-out-of-capacity/)

## Steps

- Go to [Virtual Cloud Networks](https://cloud.oracle.com/networking/vcns), select the VCN which is used for the VM you want to install Nginx there.

{%
    include image.html
    year='2022'
    month='02'
    file='25_001.png'
    alt='Virtual Cloud Networks'
%}

- Select **Security List** on the **Resources** menu.

{%
    include image.html
    year='2022'
    month='02'
    file='25_002.png'
    alt='Virtual Cloud Networks'
%}

- Select the **Security List** you want to edit.

{%
    include image.html
    year='2022'
    month='02'
    file='25_003.png'
    alt='Virtual Cloud Networks'
%}

- Click **Add Ingress Rules** then add new rule for port 80 and 443.

{%
    include image.html
    year='2022'
    month='02'
    file='25_004.png'
    alt='Virtual Cloud Networks'
%}

- Now, login to your VPS over SSH and execute the following commands to open port 80 in iptables.

- Install **firewalld**.

```terminal
sudo apt-get install firewalld
sudo systemctl enable firewalld
sudo systemctl start firewalld
```

- Open port 80.

```terminal
sudo firewall-cmd --zone=public --permanent --add-port=80/tcp
sudo firewall-cmd --reload
```

- Then verify if the port 80 is really open.

```terminal
sudo firewall-cmd --list-all
```

```terminal
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: dhcpv6-client ssh
  ports: 80/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

- Now you can install Nginx.

```terminal
sudo apt update
sudo apt install nginx
```

{%
    include image.html
    year='2022'
    month='02'
    file='25_005.png'
    alt='Nginx'
%}



