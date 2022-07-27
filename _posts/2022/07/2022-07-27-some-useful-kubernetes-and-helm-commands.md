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

- Get port of a pod

```terminal
kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}
```

- Port forwarding

```terminal
kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
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
