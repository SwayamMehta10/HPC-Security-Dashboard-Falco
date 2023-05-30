# Falco
The deployment and lifecycle of Falco in a Kubernetes cluster is managed through a Helm chart:
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

These configurations can be set even at a later stage by modifying the **/etc/falco/falco.yaml** file.

This spins up falco agents on each node in our cluster. Once the falco pods are up and running, we can view the falco logs:
```
$ kubectl logs [falco-pod-name]
```

You can checkout the <a href="https://github.com/falcosecurity/falco">official documentation</a> for more details and features.

**Bonus:** Check out <a href="https://github.com/falcosecurity/falcosidekick">falcosidekick</a> to connect falco to your ecosystem.

## Falco Rules
SSH into our node and check out the default falco rules in **/etc/falco/falco_rules.yaml**.

**Note:** It is recommended that we do not change this file but override what we need on the **/etc/falco/falco_rules.local.yaml**. 

Let us say that we do care when the history of the super-user (root) is overridden but everyone else is fine. In this case, we can append a condition to the original rule:
```
- rule: Delete or rename shell history
  append: true
  condition: and user.name=root
```
We can tell Falco to check the original rules and our overrides together:
```
# falco --validate /etc/falco/falco_rules.yaml --validate /etc/falco/falco_rules.local.yaml
```

To learn more about <a href="https://falco.org/docs/rules/">falco rules</a> and <a href="https://falco.org/docs/alerts/">alerts</a>, you can check out Sysdig's official documentation.

Lastly, we ned the <a href="falco-exporter.md">falco-exporter</a> to share the Falco alerts with the Prometheus scraper.