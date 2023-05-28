# Prometheus
Enter the following commands in terminal to install prometheus:
```
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo update
$ helm install prometheus prometheus-community/prometheus
```
By default this chart installs additional, dependent charts:
<ul>
<li>alertmanager</li>
<li>kube-state-metrics</li>
<li>prometheus-node-exporter</li>
<li>prometheus-pushgateway</li>
</ul>
You can refer to the <a href="https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus">official guide</a> to disable these and change the default configuration.

# Grafana
Grafana can be installed using the following commands:
```
$ helm repo add grafana https://grafana.github.io/helm-charts
$ helm repo update
$ helm install grafana grafana/grafana
```

# Port-Forwarding
Check whether all the pods are running:
```
$ kubectl get pods
```
We'll be using the ports 9090 and 3000 on our localhost for prometheus and grafana respectively. 
Enter the following commands in two separate terminal windows:
```
$ kubectl port-forward [prometheus-pod-name] 9090:9090
$ kubectl port-forward [grafana-pod-name] 3000:3000 
```
Open any web browser and visit **localhost:9090** and **localhost:3000** to access Prometheus and Grafana UI respectively.

To get the Grafana admin credentials:
```
$ kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

``` 

Next, add the Prometheus data source to Grafana (use **prometheus-server:80** URL)