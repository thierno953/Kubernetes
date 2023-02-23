# Deployment

- Is a resource object in kubernetes that provides declarative updates to applications.

- Allows to describe an applications lifecyle like application image, number of pods, and the way to have it updated.

- Ensures the desired number of pods are running at any given point of time, records the versioning and allows to rollback to previous versions.

### Current State ---> Desired State ---> Control Loop.

### Use Case of Kubernetes deployment

- Create deployment to rollout a ReplicaSet
- Declare the new state of the Pods by updating the PodTemplateSpec of the deployment
- Rollback to an earlier deployment revision
- Scale up the deployment to facilitate more load

```bash
-------------Deployment---------------------------------
    |                                        |
ReplicaSet1                             ReplicaSet2
    |                                        |
-----------------                       -----------------
pod1 |pod2 | pod3                       pod1 |pod2 | pod3
```

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginxdeploy
  labels:
    name: nginxdeploy
spec:
  replicas: 10
  minReadySeconds: 30
  strategy:
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 0
    type: RollingUpdate
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: dpod
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

- explain deployment

```bash
kubectl explain deployment
```

- create the Deployment

```bash
kubectl apply -f deploymentmanifest.yml
```

- check deployment

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

```bash
kubectl get rs
```

- Get details of your Deployment

```bash
kubectl describe deployment/nginxdeploy
```

```bash
kubectl delete pod <NAME>
kubectl get pods --show-labels
```

- rollout status

```bash
kubectl rollout status deployment/nginxdeploy
```

- check the revisions of this Deployment

```bash
kubectl rollout history deployment/nginxdeploy
```

- delete deployment

```bash
kubectl delete deployment/nginxdeploy
```
