```
kubectl create secret docker-registry private-registry-creds \
    --docker-username=docker_user \
    --docker-password=docker_password \
    --docker-server=dockerprivateregistry.com:5000 
    --docker-email=docker_user@dockerprivateregistry.com
```

Sample output:
```
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJteXByaXZhdGVyZWdpc3RyeS5jb206NTAwMCI6eyJ1c2VybmFtZSI6ImRvY2tfdXNlciIsInBhc3N3b3JkIjoiZG9ja19wYXNzd29yZCIsImVtYWlsIjoiZG9ja191c2VyQG15cHJpdmF0ZXJlZ2lzdHJ5LmNvbSIsImF1dGgiOiJaRzlqYTE5MWMyVnlPbVJ2WTJ0ZmNHRnpjM2R2Y21RPSJ9fX0=
kind: Secret
metadata:
  name: private-registry-creds
type: kubernetes.io/dockerconfigjson
```

Use in a pod:
```
(...)
spec:
    imagePullSecrets:
    - name: private-registry-creds
    containers:
    - image: dockerprivateregistry.com:5000/nginx:alpine
    imagePullPolicy: IfNotPresent
    name: nginx
```