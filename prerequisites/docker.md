# Install Docker Engine on Ubuntu
This file contains all the necessary steps required for installing Docker Engine on Ubuntu. 

## Uninstall old versions
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Install using the apt repository
#### Set up the Docker repository using the following commands:
```
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl gnupg
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
#### Install Docker Engine, containerd and Docker Compose:
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
$ sudo docker run hello-world
```

For other installation methods or resolving errors you can refer to the <a href="https://docs.docker.com/engine/install/ubuntu/">official documentation</a>.

Alternatively you can also install <a href="https://docs.docker.com/desktop/install/ubuntu/">Docker Desktop</a> on Ubuntu.

Next, we need to install <a href="..\setup.md#minikube">minikube</a>.