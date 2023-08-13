---
title: "How to install .Net 6 on Linux Arm?"
date: 2022-02-26 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: dotnet
tags: .net arm
---

## Intro
In this post I would like to document how I install .Net 6 on Linux Arm64 manually. For detailed instructions, you can read on [Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/install/).

## Steps

- Go to download site of [.Net 6](https://dotnet.microsoft.com/en-us/download/dotnet/6.0). Download the version for your server to local server. In my case I downloaded the **Arm64** version and save the file as **dni.tar.gz**.

```terminal
curl https://download.visualstudio.microsoft.com/download/pr/d30e9e2e-10a5-46c6-9b81-1a6a965d6739/6f56a3893b16fa841c8cb34ec9b3768d/aspnetcore-runtime-6.0.2-linux-arm64.tar.gz --output dni.tar.gz
```

{%
    include image.html
    year='2022'
    month='02'
    file='26_001.png'
    alt='Arm'
%}

- Create **/usr/share/dotnet**

```terminal
mkdir -p /usr/share/dotnet
```

- Extract **dni.tar.gz** (downloaded from Microsoft) to **/usr/share/dotnet**.

```terminal
tar zxf dni.tar.gz -C /usr/share/dotnet/
```

- Create a symbolic link to **/usr/bin/dotnet**

```terminal
ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
```

- Test if **dotnet** command can be found

```terminal
dotnet --info
```

- Sample config for using **supervisor** to restart application

```terminal
sudo apt-get install supervisor
sudo nano /etc/supervisor/conf.d/{domain}.conf
```

```terminal
[program:APP_NAME]
command=/usr/bin/dotnet API_DLL_PATH --urls http://0.0.0.0:API_PORT
directory=API_DIR_PATH
autostart=true
autorestart=true
stderr_logfile=/var/log/API_DOMAIN.err.log
stdout_logfile=/var/log/API_DOMAIN.out.log
environment=ASPNETCORE_ENVIRONMENT=Production
user=www-data
stopsignal=INT
```

Restart **supervisor**

```terminal
sudo service supervisor restart
sudo tail -f /var/log/supervisor/supervisord.log
```

or 

```terminal
supervisorctl restart canduyentiendinh
```