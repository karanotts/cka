Static pods are not dependant on apiserver


`kubectl run --restart=Never --image=busybox:1.28.4 static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml`


Default location: `/etc/kubernetes/manifests`

Specify custom location in `kubelet.service`, config file `kubeconfig.yaml` or var file `/var/lib/kubelet/kubeadm-flags.env`

```
systemctl cat kubelet.service
ExecStart=/usr/bin/kubelet \\
--container-runtime=remote \\
--container-runtime-endpoint=/run/containerd/containerd.sock \\
--pod-manifest-path=/etc/kubernetes/manifests
```

alternatively, set config file in service:
```
systemctl cat kubelet.service
ExecStart=/usr/bin/kubelet \\
--container-runtime=remote \\
--container-runtime-endpoint=/run/containerd/containerd.sock \\
--config=kubeconfig.yaml

cat kubeconfig.yaml
staticPodPath: /etc/kubernetes/manifests
```

or var file in service:

```
systemctl cat kubelet.service
ExecStart=/usr/bin/kubelet \\
$KUBELET_KUBEADM_ARGS \\

cat /var/lib/kubelet/kubeadm-flags.env
KUBELET_KUBEADM_ARGS="--container-runtime=remote --container-runtime-endpoint=/run/containerd/containerd.sock --pod-manifest-path=/etc/kubernetes/manifests"
```