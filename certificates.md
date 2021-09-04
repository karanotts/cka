`cat /etc/kubernetes/manifests/kube-apiserver.yaml```
```
(...)
 containers:
  - command:
    - kube-apiserver
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
    - --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
    - --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
    - --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
    - --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
```

Look up the cert data:
```openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout```



## Add user

Encode certificate signing request:
```
cat myuser.csr | base64
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBN
QTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJD
Z0tDQVFFQXZKK3F4aDJjTWJPU3Q3dU9LQmJCNlA1V1loV09pbTBpU0duRHNNT2VRWEJECkFYeFBn
ci80NXozYTFhdHduMzgvSy9pVGVwRnludXphWGJkbFBlQ1dqMldhSGpqWURWAWODIJAOWNLAWIv=
```

Add encoded csr to manifest:
```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: myuser
spec:
  signerName: kubernetes.io/kube-apiserver-client
  groups:
  - system:authenticated
  usages:
  - client auth
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQXZKK3F4aDJjTWJPU3Q3dU9LQmJCNlA1V1loV09pbTBpU0duRHNNT2VRWEJECkFYeFBnci80NXozYTFhdHduMzgvSy9pVGVwRnludXphWGJkbFBlQ1dqMldhSGpqWURWAWODIJAOWNLAWIv=
```

View certificate signing requests:

```
kubectl get csr
NAME        AGE   SIGNERNAME                                    REQUESTOR                  CONDITION
myuser      37s   kubernetes.io/kube-apiserver-client           kubernetes-admin           Pending
```

Approve certificate signing requests:
```
kubectl certificate approve myuser
certificatesigningrequest.certificates.k8s.io/myuser approved
```

Deny certificate signing requests:
```
kubectl certificate deny rogue-user
certificatesigningrequest.certificates.k8s.io/rogue-user denied
```