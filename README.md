# HPC-Security-Dashboard-Falco
This repository contains all the necessary files and steps required for setting up Falco runtime security tool for Kubernetes. 

**Note:** *All the installation steps mentioned are for Ubuntu OS since Falco is a Linux specific tool.*

*I would strongly recommend using a **Virtual Machine** to run Ubuntu on Windows OS instead of using WSL. Using WSL will require a large number of extra steps for setting up Docker and Falco.*

## Docker driver
We'll be using the <a href="prerequisites\docker.md">Docker driver</a> to start a minikube cluster. 

## minikube
This is the easiest way to use Falco on Kubernetes in a local environment.  

To install the latest minikube **stable** release on **x86-64 Linux** using **binary download**:
```
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
To make docker the default driver:
```
$ minikube config set driver docker
```
From a terminal with administrator access (but not logged in as root), run:
```
$ minikube start
```

To know more about interacting with your cluster, deploying applications and managing your cluster, you can refer to the <a href="https://minikube.sigs.k8s.io/docs/start/">official documentation</a>.