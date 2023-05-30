# Generating Simple Falco Events
We will run a few commands that will cause Falco to record some warnings and alerts. Log into the minikube environment before generating the events:
```
$ minikube ssh
```

**Note:** If any of these commands do not work (permission denied), try executing them as sudo-user
## Run a Privileged Container
```
$ docker run --rm --interactive --tty --privileged --volume /etc/shadow:/mnt/shadow fedora:latest ls -l /mnt/shadow
``` 
The falgo log should show an informational event:
<img src="images\img-1.png">