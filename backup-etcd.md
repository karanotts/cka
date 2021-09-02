<ol>
<li>take a snapshot of etcd</li>

```
ETCDCTL_API=3 etcdctl snapshot save <snapshot-file>
```

<li>stop kube-apiserver (if applicable, in stacked etcd)</li>

```
service kube-apiserver stop
```

<li>restore from snapshot</li>

```
ETCDCTL_API=3 etcdctl 
  --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> \
  --cert=<cert-file> \
  --key=<key-file> \
  snapshot restore <snapshot-file>
```

 if etcd database is TLS-Enabled, the following options are mandatory:

```--cacert``` - verify certificates of TLS-enabled secure servers using this CA bundle

```--cert``` - identify secure client using this TLS certificate file

```--endpoints=[127.0.0.1:2379]``` - default if etcd is running on master node and exposed on localhost 2379.

```--key``` - identify secure client using this TLS key file

<li>restart services</li>

```
systemctl daemon-reload
service etcd restart
service kube-apiserver start
```

<li>recommended: restart kube-scheduler, kube-controller-manager, kubelet</li>
</ol>