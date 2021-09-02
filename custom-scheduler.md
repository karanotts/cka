`- --scheduler-name=kustom-kube-scheduler`

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: kube-scheduler
    tier: control-plane
  name: kustom-kube-scheduler
  namespace: kube-system
spec:
  containers:
  - name: kustom-kube-scheduler
    command:
    - kube-scheduler
    - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
    - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
    - --bind-address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=true
    - --port=0
    - --scheduler-name=kustom-kube-scheduler
    image: k8s.gcr.io/kube-scheduler:v1.20.0
    imagePullPolicy: IfNotPresent
    (...)
```

Assign pod to `kustom-kube-scheduler`:

```
apiVersion: v1
kind: Pod
metadata:
  name: kustom-pod
spec:
  containers:
  - image: nginx
    name: nginx
  schedulerName: kustom-kube-scheduler
```