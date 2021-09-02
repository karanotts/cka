## Draining nodes 
Draining a node moves pods to other available nodes and marks node unschedulable.

```kubectl drain node01 --ignore-daemonsets```
```
node/node01 cordoned
```

(DaemonSets run all nodes by design, so ```--ignore-daemonsets``` flag is likely be required)

```
kubectl get nodes
NAME           STATUS                     ROLES                  AGE   VERSION
controlplane   Ready                      control-plane,master   45m   v1.20.0
node01         Ready,SchedulingDisabled   <none>                 44m   v1.20.0
```

After performing maintenance tassk, make node schedulable again:

```kubectl uncordon node01```
```
node/node01 uncordoned
```

Draining works for pods that are part of deployment/replicaset. Doesn't work on standalone pods, if drain is forced, these will be lost.
To make node unschedulable without eviciting running pods, use:

```kubectl cordon node01```


## Upgrading with kubeadm

1) drain controlplane:

```
kubectl drain controlplane --igore-daemonset
```

2) upgrade ```kubeadm```:
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.20.2-00
```

3) plan upgrade:
```
sudo kubeadm upgrade plan v1.20.2
```

4) upgrade controlplane:
```
sudo kubeadm upgrade apply v1.20.2
```

5) upgrade ```kubelet``` and ```kubectl``` on the controlplane:
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.20.2-00 kubectl=1.20.2-00
```

6) restart ```kubelet```:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

7) uncordon controlplane:
```
kubectl uncordon controlplane
```

After successfulling upgrading controlplane, move on to upgrading worker nodes:
1) drain worker node:
```
kubectl drain node01 --ignore-daemonsets
```

2) log in to worker node and upgrade ```kubeadm```:
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubeadm=1.20.2-00
```

3) upgrade ```kubelet``` config on worker node:
```
sudo kubeadm upgrade node
```

4) upgrade ```kubelet``` and ```kubectl``` on worker node:
```
sudo apt-get update && \
sudo apt-get install -y --allow-change-held-packages kubelet=1.20.2-00 kubectl=1.20.2-00
```

5) restart ```kubelet``` on worker node:
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

6) uncordon worker node:
```
kubectl uncordon node01
```