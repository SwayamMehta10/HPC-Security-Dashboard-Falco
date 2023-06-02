# Generating Simple Falco Events
We will run a few commands that will cause Falco to record some warnings and alerts. Log into the minikube environment before generating the events:
```
$ minikube ssh
```

**Note:** If any of these commands do not work (permission denied), try executing them as sudo-user

## Run a privileged container
```
$ docker run --rm --interactive --tty --privileged --volume /etc/shadow:/mnt/shadow fedora:latest ls -l /mnt/shadow
``` 
The falgo logs should show an informational event:
![VirtualBox_myVM_30_05_2023_12_43_38](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/c98bae16-d3d2-4670-b4b1-22f7fe5b0d12)

## Create a file on the /root directory
```
$ docker run --rm --interactive --tty --user root --volume /root:/mnt/ fedora:latest touch /mnt/test_file
```
Falco should detect a sensitive mount:
![VirtualBox_myVM_30_05_2023_12_53_57](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/82f2ba6b-91ea-4717-b96c-4f65a6f68952)

## Creating a file on /bin
```
$ sudo -i
# touch /bin/should_not_be_here
```
This should result in an error stating that a binary directory has been opened for writing:
![VirtualBox_myVM_30_05_2023_12_53_57 (1)](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/3c98b236-7463-46f4-a353-89bb1fe8a7f9)

## Generating a few events on loop
```
$ for i in $(seq 1 60); do docker run --rm --interactive --tty --privileged fedora:latest /bin/bash -c ls; touch /root/test; rm -f /root/test; sleep 1; done
```
Falco logs:
![VirtualBox_myVM_30_05_2023_12_47_56](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/1ad4b990-de46-4f25-a2c8-83e484a841db)

## Terminal shell in container
In development, integration, and staging environments, accessing a container and manually running commands is something that occurs pretty frequently and probably shouldn't raise an alert. However, the fact that this is occurring in your production environment may indicate a breach, particularly if you are unable to pinpoint an event time.

Let us add Helm's stable repository and install **mysql-db** chart from it:
```
$ helm repo add stable https://charts.helm.sh/stable
$ helm repo update
$ helm install mysql-db stable/mysql
```
Wait till the mysql-db pod starts running and then open a terminal shell on the pod:
```
$ kubectl exec -it [mysql-db-pod-name] -- bash -il
```
Falco should be able to detect this:
![VirtualBox_myVM_30_05_2023_12_51_08](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/d26abd4f-8203-4a13-89fb-ea151e3c232b)

## Contact Kubernetes API Server from Container
Falco can also keep track of the requests operators and developers make to the Kubernetes API. You can create rules that instruct it to keep an eye out for operations that are obviously hazardous, like giving new users cluster-admin capabilities or making ConfigMaps with useful data.

To detect attempts to contact the Kubernetes API Server from a container, we'll run a tool called **kube-recon** which does initial reconnaissance in a K8s environment:
```
$ kubectl run kuberecon --tty -i --image octarinesec/kube-recon
/# ./kube-recon
```
Detection:
![VirtualBox_myVM_30_05_2023_12_58_09](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/2f78a86d-be37-4ae4-81b0-fea88959e6f6)
