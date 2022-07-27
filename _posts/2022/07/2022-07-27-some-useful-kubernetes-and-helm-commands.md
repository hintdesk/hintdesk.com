---
title: "Some useful Kubernetes and Helm commands"
date: 2022-07-27 00:00:00:00 +00:00
author: tri
layout: post
image: /assets/img/2021/06/deep-fried.png
icon: k8s
tags: kubernetes
---

## Intro
In this post I would like to list some useful commands for using Kubernetes.

## Kubernetes
Ref: [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- Get cluster info

```terminal
kubectl cluster-info
```

- List all pods

```terminal
kubectl get po
```

- List all deployments

```terminal
kubectl get deploy
```


## Helm

- List all installtions

```terminal
helm list
```

- Make an installation

```terminal
helm install {release-name} {folder}
```


## Rancher Desktop

- Set proxy for Rancher Desktop

```terminal
setx HTTP_PROXY http://your_proxy_server
setx HTTPS_PROXY https://your_ssl_proxy_server
setx WSLENV HTTP_PROXY:HTTPS_PROXY
```

Restart Rancher Desktop and check if the proxy has been applied

```terminal
wsl -d rancher-desktop echo ${HTTP_PROXY}
wsl -d rancher-desktop echo ${HTTPS_PROXY}
```





