## Scenario:
Create a network policy to allow traffic from the Internal application only to the payroll-service and db-service.


```
kubectl get pods --show-labels
NAME       READY   STATUS    RESTARTS   AGE   LABELS
external   1/1     Running   0          15m   name=external
internal   1/1     Running   0          15m   name=internal
mysql      1/1     Running   0          15m   name=mysql
payroll    1/1     Running   0          15m   name=payroll
```

