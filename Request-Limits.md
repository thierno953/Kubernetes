# Resource Limits

**Request**<br>
**Limits**

```shell
Limit
2.0 cpu  =======> #Container never goes above the value

Request
0.5 cpu  =======> #Container is guaranteed to get
```

```shell
apiVersion: v1
kind: Pod
metadata:
  name: web
  namespace: cpu-example
spec:
  containers:
  - name: nginx
    image: web
    resources:
      requests:
        memory: "28Mi"
        cpu: "250m"
      limits:
        memory: "28Mi"
        cpu: "250m"
```

```bash
kubectl apply -f cpu_requests.yml

kubectl get ns

kubectl get pods --namespace cpu-example

kubectl top pod web --namespace cpu-example

kubectl delete pod web --namespace cpu-example
```
