# Falco
```
$ helm repo add falcosecurity https://falcosecurity.github.io/charts
$ helm repo update
```

We'll be using the **ebpf probe** and enabling **gRPC** over a Unix socket.
We'll also enable the **json output** to print falco alert messages as json.

```
$ helm install falco \
--set driver.kind=ebpf \
--set tty=true \
--set json_output=true \
--set falco.grpc.enabled=true \
--set falco.grpc_output.enabled=true
```

This spins up falco agents on each node in our cluster. Once the falco pods are up and running, we can view the falco logs:
```
$ kubectl logs [falco-pod-name]
```

You can checkout the <a href="https://github.com/falcosecurity/falco">official documentation</a> for more details and features.

**Bonus:** Check out <a href="https://github.com/falcosecurity/falcosidekick">falcosidekick</a> to connect falco to your ecosystem.

Next, we need to install the <a>falco exporter</a> to export Prometheus Metrics for Falco output events.