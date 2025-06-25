---
title: "What does eval $(minikube docker-env) do??"
date: 2025-06-20T00:06:44+08:00
draft: false
author: 'Yang'
tags: ['tree', 'DFS']
categories: ['leetcode']
---

### Why do I need this command
Recently I'm following a [k8s tutorials](https://github.com/guangzhengli/k8s-tutorials), which is for the beginners who just start getting into the world of kubernetes. To create a lightweight cluster for education purpose, this tutorial adopt `minikube` as the container orchestration tool, which will run a full k8s to manage the containers created inside the cluster. One really important thing to notice that is that **the `minikube` cluster itself is a docker container running on my local machine**.

In this tutorial, a special Docker container runs on local Docker deamon, which acts as a mini VM which contains everything to run K8s:

`minikube start --vm-driver=docker --container-runtime=docker`

And the base image for the VM is:
`gcr.io/k8s-minikube/kicbase`, which contains:
- A minimal Linux OS

- Kubernetes components: kubelet, kube-apiserver, etc.

- A container runtime (like Docker or containerd)

### How do we usually communicate with Docker
For my default usage of Docker, I have a docker CLI allowing me to set prompt to talk to docker daemon through terminal. Then docker will start running any required images on my local machine.

### How to communicate with Docker inside the mini VM?
Since docker is selected as the runtime running inside minikube, I need to find a way to talk to Docker daemon in order to manage containers in it.

The command temporarily configuring local docker CLI(terminal) to talk to that inner Docker daemon — the one running inside the Minikube container:

`eval $(minikube docker-env)`

Any container running inside the minikube container should be accessed through the minikube ip:

*Get the ip of VM:*
`minikube ip` -> `192.168.49.2`

*Access the container inside the cluster:*
`curl 192.168.49.2:3000`

Now we are able to communicate with the docker Daemon inside minikube container through the terminal on local machine.


### What does this command do?
This command lets the Docker CLI talk to the Docker daemon inside Minikube instead of my local one.

Step-by-step:
1. `minikube docker-env` outputs shell commands like:

```
export DOCKER_HOST=...
export DOCKER_CERT_PATH=...
```

1. eval $(...) runs those export commands in the current shell.

2. The docker commands now control containers inside Minikube, not on host.

**To swich back to local docker, simply go with:
`eval $(minikube docker-env -u)` , or just start a new terminal.**