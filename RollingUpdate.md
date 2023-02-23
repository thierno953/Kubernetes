# Rolling Update Deployment

- Kubernetes Rolling Update strategy is to replace old pods with new ones gradually, while continuning to serve clients without incurring downtine.<br>
  **RollingUpdate**: New pods are added gradually, and old pods are terminated gradually.<br>
  **Recreate**: All old pods are terminated before any new pods added.

Deployment & Service

```bash
         Service
            |
          ------
        Deployment
            |
          ------
        Replicaset
        ----------
            |
------------------------------------
|          |            |          |
Pod 1     Pod 2        Pod 3      Pod 4
```

Downtime Deployment

```bash
   Service
      |
      |
--------------------------------------------------
|                  |             |               |
nginx 1.22     nginx 1.22     nginx 1.22        nginx 1.22

Initial Stage: Nginx rolling update deployment
```

```bash
   Service
      |
      |
--------------------------------------------------
|                 |                |              |
nginx 1.22   nginx 1.22     #nginx 1.22    #nginx 1.22
                                   |              |
                            nginx 1.22.3    nginx 1.22.3

Turn Off old pods and create new version pods
```

```bash
   Service
      |
      |
--------------------------------------------------
|               |             |             |
nginx 1.22   nginx 1.22     nginx 1.23.3    nginx 1.23.3

Serving both versions of Nginx
```

```bash
   Service
      |
      |
--------------------------------------------------
|                 |                |              |
# nginx 1.22   #nginx 1.22     #nginx 1.22.3    #nginx 1.22.3
                                   |              |
                              nginx 1.22    nginx 1.22

Further update
```

```bash
   Service
      |
      |
--------------------------------------------------
|                 |                |              |
nginx 1.22.1   nginx 1.22.1     nginx 1.22.1    nginx 1.22.1


Final Stage: Update complet
```

### Rolling Update Strategy

```bash
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
          successThreshold: 1

strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 50%
    maxUnavailable: 50%
```

- MaxUnavailable: The maximum number of pods that can be unavailable during the update process. This can be an absolute or percentage of the replicas count.
- MaxSurge: The maximum number of pods that can be created over the desired number of pods. Again this can be an absolute number or a percentage of the replicas count.
- ReadinessProbe for your service container to help kubernetes determine the state of the pods. Kubernetes will only route the client traffic to the pods with a healthy liveness probe.
- InitialDelaySeconds: Number of seconds after the container has started before readiness probes are initiated.
- PeriodSeconds: How ofter (in seconds) to perform the probe. Default to 10 seconds. Minimum value is 1.
- TimeoutSeconds: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.
- SuccessThreshold: Minimum consecutive success for the probe to be considered successful after having failed. Defaults to 1. Minimum value is 1.

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-rolling-update
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        role: rolling-update
    spec:
      containers:
        - image: nginx:1.22.1
          imagePullPolicy: Always
          name: nginx-container
          ports:
            - containerPort: 80
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 5
            successThreshold: 1

  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 50%
      maxUnavailable: 50%

---
kind: Service
apiVersion: v1
metadata:
  name: service-rolling-update
  labels:
    app: nginx
    role: rolling-update
    env: prod
spec:
  selector:
    app: nginx
    role: rolling-update
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 80
      nodePort: 31110
```

```bash
kubectl apply -f rollingupdate.yml

kubectl get deployments

kubectl get pods

kubectl get svc

minikube service <NAME>

kubectl describe deployment <NAME>

kubectl describe pod <NAME>
```
