check node status
```kubectl get nodes```

check controlplane pods
```kubectl get pods -n kube-system```

check controlplane services
```service kube-apiserver status```
```service kube-controller-manager status```
```service kube-scheduler status```
```service kubelet status```
```service kube-proxy status```

check service logs
```kubectl logs kube-apiserver-master -n kube-system```
```sudo journalctl -u kube-apiserver```