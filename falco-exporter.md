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
We can check the metrics endpoint at **localhost:9376/metrics**. This is what the falco metrics look like:
![VirtualBox_myVM_26_05_2023_02_17_39](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/d3172c9f-4de7-4718-8472-6184df2f14da)
![VirtualBox_myVM_26_05_2023_02_18_25](https://github.com/SwayamMehta10/HPC-Security-Dashboard-Falco/assets/79704715/7e4bfecb-6df5-4275-8527-5d0a1d2f958d)

Now, we just need to <a href="setup.md#falco-dashboard"> import the Falco dashboard</a> to Grafana.
