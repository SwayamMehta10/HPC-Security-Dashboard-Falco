# Falco-Exporter
We'll be deploying falco-exporter using the helm chart:
```
$ helm repo add falcosecurity https://falcosecurity.github.io/charts
$ helm repo update
$ helm install falco-exporter falcosecurity/falco-exporter
```
For other installation methods and custom configuration, you can checkout the <a href="https://github.com/falcosecurity/falco-exporter">official guide</a>.

Make sure all the pods are running:
```
$ kubectl get pods
```

Prometheus automatically starts capturing falco events once the falco-exporter starts running.

We'll be forwarding the falco-exporter metrics to port 9376. In a new terminal tab:
```
$ kubectl port-forward [falco-exporter-pod-name] 9376:9376 
```
We can check the metrics endpoint at **localhost:9376/metrics**