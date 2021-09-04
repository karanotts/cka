```kubectl auth can-i delete pods```

```kubectl get pods --as dev-user```


```
kubectl create role developer --verb=list,create --resource=pods --dry-run=client -o yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: null
  name: developer
rules:
- apiGroups:
  - ""
  resources:
  - pods
  verbs:
  - list
  - create
```

```
kubectl create rolebinding dev-user-binding --role=developer --user=dev-user --dry-run=client -o yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: dev-user-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: developer
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: dev-user
```


```kubectl get role kube-proxy -n kube-system -o yaml```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  managedFields:
  - apiVersion: rbac.authorization.k8s.io/v1
    manager: kubeadm
    operation: Update
    time: "2021-09-02T15:02:59Z"
  name: kube-proxy
  namespace: kube-system
  resourceVersion: "243"
  uid: 9954cd49-3464-40d1-8a53-23e07124bea9
rules:
- apiGroups:
  - ""
  resourceNames:
  - kube-proxy
  resources:
  - configmaps
  verbs:
  - get
```


```kubectl get rolebinding kube-proxy -n kube-system -o yaml```
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: "2021-09-02T15:02:59Z"
  managedFields:
  - apiVersion: rbac.authorization.k8s.io/v1
    manager: kubeadm
    operation: Update
    time: "2021-09-02T15:02:59Z"
  name: kube-proxy
  namespace: kube-system
  resourceVersion: "244"
  uid: 8cc1d7e8-9345-46b7-858e-a3856dc9ba10
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: kube-proxy
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:bootstrappers:kubeadm:default-node-token
  ```