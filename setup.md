# Setup
This repository contains all the necessary files and steps required for setting up Falco runtime security tool for Kubernetes. 

**Note:** *All the installation steps mentioned are for Ubuntu OS since Falco is a Linux specific tool.*

*I would strongly recommend using a **Virtual Machine** to run Ubuntu on Windows OS instead of using WSL. Using WSL will require a large number of extra steps for setting up Docker and Falco.*

## Docker driver
We'll be using the <a href="prerequisites\docker.md">Docker driver</a> to start a minikube cluster. 

## Minikube
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

**Optional:** You can install <a href="https://k9scli.io/topics/install/">k9s</a> which is a terminal based UI for easy interaction with Kubernetes clusters.

**Important:** Make sure that <a href="https://helm.sh/">helm</a> is installed before proceeding further.

## Prometheus and Grafana
We'll be using the helm charts provided by *Prometheus Monitoring Community* and *Grafana Labs* to deploy these services into our minikube cluster.

Once the minikube cluster starts, follow these <a href="prerequisites\prometheus grafana.md">installation steps</a> for Prometheus and Grafana.

## Falco
Once all the pods are up and running, we can start the <a href="falco.md">falco installation</a>.

To access the installed falco files, we need to log into the minikube environment:
```
$ minikube ssh
```

## Falco Exporter
Once Falco is installed and running with the gRPC output enabled, we can install the <a href="falco-exporter.md">falco-exporter</a>.

## Falco Dashboard
We can now import the Falco dashboard to Grafana. A custom dashboard has been created and the JSON file for it has been provided in this repository.

The customized dashboard can be imported to Grafana by following these steps:
1. Download the <a href="falco dashboard.json" download>JSON</a> file for the custom Falco dashboard provided in this repository. 
2. Once logged into Grafana, click on **Dashboards** in the left-side menu.
3. Click on **new** and select **Import** in the dropdown menu.
4. Upload the Falco dashboard JSON file **OR** Paste the dashboard JSON text directly into the text area.

To learn more about managing your dashboard, you can visit <a href="https://grafana.com/docs/grafana/latest/dashboards/">Grafana Labs</a>.

## Generating Events
I have provided a few ways to <a href="generate events.md">generate sample events</a> that trigger the Falco agent. The results are reflected on the dashboard in real-time.

You can also check out the <a href="https://falco.org/docs/event-sources/sample-events/">event-generator tool</a> provided in the official Falco docs. 
