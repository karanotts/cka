check node status
```kubectl get nodes```

check node resources
```top```
```df -h```

check kubelet status
```service kubelet status```
```systemctl cat kubelet.service```
```sudo journalctl -u kubelet```

check certificates
```openssl x509 -in /var/lib/kubelet/node-1.crt -text```