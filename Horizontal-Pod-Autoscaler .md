# Horizontal Pod Autoscaler (HPA)

### How does the horizontal pod autoscaler work?

- Horizontal Pod Autoscaler automatically scales the number of Pods in a replication controller, deployment, replicaset or stateful set based on observed CPU utilization.
- Horizontal Pod Autoscaling does not apply to objects that can't be scale like DaemonSets.
- The controller periodically adjusts the number of replicas in a replication controller or deployment to match the observed metrics such as average CPU utilization, average memory utilization or any other custom metric.

### Horizontal Pod Autoscaling

```bash
Pod 1 ---------------Pod 2------------------Pod N
                      |
               ------------------
                | RC / Deployment|
                |   Scale        |
                ------------------
                      |
                -----------------
                |  Horizontal   |
                |    Pod        |
                | Autoscaler    |
                -----------------
```

- HPA will increase and decrease the number of replicas to maintain an average CPU utilization across all Pods as per the defined value (e.g.: 50%).

to create a file

**vim hpanginx.yml**

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploymentnginx
  namespace: default
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.7.9
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: "250m"

---
apiVersion: v1
kind: Service
metadata:
  name: nginxservice
  labels:
    app: nginx
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      nodePort: 31050
      targetPort: nginxwebport
```

```bash
kubectl apply -f hpanginx.yml
```

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

```bash
kubectl get svc
```

========================

**vim hpanginx.yml**

```shell
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: hpanginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: deploymentnginx
  minReplicas: 4
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

```bash
kubectl explain hpa
```

```bash
kubectl apply -f hpanginx.yml
```

```bash
kubectl get hpanginx
```

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

========================
**NB**:

```shell
kubectl autoscale deployment deploymentnginx --cpu-percent=50 --min=1 --max=10
```

**vim hpabeta.yml**

```shell
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
    - type: Resource
      resource:
        name: memory
        target:
          type: AverageValue
          averageValue: 100Mi
```

```bash
kubectl get hpa

kubectl describe hpa hpanginx

kubectl delete hpa hpanginx

kubectl get deployments

kubectl delete deployment deploymentnginx

kubectl get svc

kubectl delete svc nginxservice
```
