# microk8s
## Master setup 
Install `dns` _first_[^1], and then the addons for monitoring with:

```
microk8s enable dns
microk8s enable dashboard prometheus
```

One can forward port from the dashboard pod to the host with:

```
microk8s dashboard-proxy
```

To access grafana, forward the port with:

```
microk8s kubectl port-forward -n monitoring service/grafana 3000:3000 --address 0.0.0.0
```

Note that by default grafana, unlike the dashboard proxy, doesn't use SSL[^2].

## Issues
### High resource usage
Having a HA cluster is very resource demanding for the 3 unfortunate nodes
which end up being the master. Since I don't have the capacity for this, I live
on the edge without `microk8s.disable ha-cluster`.

### `flanneld` failing on a node
Happened to me when adding one node.

0. Call IT
1. Exit the cluster with `microk8s leave`
2. Remove the node on master with `microk8s remove node {hostname}`
3. Turn HA on the node with `microk8s.enable ha-cluster`
4. Turn it off again with `microk8s.disable ha-cluster`
5. Reconnect
6. Thank you Roy

- [https://discuss.kubernetes.io/t/solved-x509-certificate-error/14151/3]

### Hostname as IP instead of name
To get hostname as node name, the node IP must be present in the `/etc/hosts`
file. See the [`setup.md`](setup.md) for list of nodes and their IPs on the
home network. 

- [https://serverfault.com/q/1059356/442421]

[^1]: Installing `prometheus` directly after `dns` results in an error _"error:
  error running snapctl: snap "microk8s" has "service-control" change in
  progress"_

[^2]: This means that when you access it via https, you get
  `ERR_SSL_PROTOCOL_ERROR` in Firefox and `SSL_ERROR_RX_RECORD_TOO_LONG` in
  Chrome, as I've learnt the hard way.

<!-- References -->
[docs]: https://microk8s.io/docs
[ha]: https://microk8s.io/docs/high-availability
[disable-ha]: https://discuss.kubernetes.io/t/high-availability-ha/11731/21
[prometheus]: https://www.server-world.info/en/note?os=Ubuntu_20.04&p=microk8s&f=8
[cpu-limits]: https://learnk8s.io/setting-cpu-memory-limits-requests
