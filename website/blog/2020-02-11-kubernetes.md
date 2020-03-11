---
layout: post
title: "Kubernetes"
description: ""
category: linux
tags: [linux, macos, ubuntu, kubernetes, docker]
---
{% include JB/setup %}

## Installation

### macOS

#### Install Docker and Kubernetes

Download [Docker Desktop](https://docs.docker.com/docker-for-mac/) and install. Don't run Docker for now.

In China, you have to do this before start k8s.

```sh
git clone https://github.com/gotok8s/k8s-docker-desktop-for-mac.git
cd k8s-docker-desktop-for-mac/
cat images
./load_images.sh
```

Run Docker.

`Prefs...` -> `Resources` -> `ADVANCED`.

Set Memory to at least 16GB, Swap to at least 4GB.

`Prefs...` -> `Docker Engine`, add this

```sh
  "registry-mirrors": [
    "https://dockerhub.azk8s.cn"
  ],
```

`Prefs...` -> `Kubernetes` -> `Enable Kubernetes` -> `Apply & Restart`.

#### Check Status

```sh
$ kubectl version
$ kubectl cluster-info
Kubernetes master is running at https://kubernetes.docker.internal:6443
KubeDNS is running at https://kubernetes.docker.internal:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

$ kubectl get nodes
NAME             STATUS   ROLES    AGE    VERSION
docker-desktop   Ready    master   5h5m   v1.15.5

$ kubectl describe node
Name:               docker-desktop
Roles:              master
...

$ kubectl get pods --all-namespaces
NAMESPACE              NAME                                         READY   STATUS    RESTARTS   AGE
docker                 compose-7b7c5cbbcc-fsxd6                     1/1     Running   0          5h24m
docker                 compose-api-dbbf7c5db-nzdht                  1/1     Running   0          5h24m
kube-system            coredns-5c98db65d4-fzbkg                     1/1     Running   1          5h25m
kube-system            coredns-5c98db65d4-vxcdt                     1/1     Running   1          5h25m
kube-system            etcd-docker-desktop                          1/1     Running   0          5h24m
kube-system            kube-apiserver-docker-desktop                1/1     Running   0          5h24m
kube-system            kube-controller-manager-docker-desktop       1/1     Running   0          5h24m
kube-system            kube-proxy-jzvrz                             1/1     Running   0          5h25m
kube-system            kube-scheduler-docker-desktop                1/1     Running   0          5h24m
kubernetes-dashboard   dashboard-metrics-scraper-7f5767668b-bqtw9   1/1     Running   0          39m
kubernetes-dashboard   kubernetes-dashboard-57b4bcc994-x97l5        1/1     Running   0          39m
```

#### Install Dashboard

[Kubernetes Dashboard](https://github.com/kubernetes/dashboard)

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-rc5/aio/deploy/recommended.yaml
kubectl proxy
```

In another shell:

```sh
kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}'
```

Copy token.

Open <http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/> in browser.

[在 macOS 上使用 Docker Desktop 启动 Kubernetes 踩坑全记录](https://juejin.im/post/5d87980f5188253f74438bb6)
