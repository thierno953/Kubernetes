# Service

Service is an abstract way of exposing an application running on a set of Pods as a network service.

- **1**: Create Nginx pod using pod manifest yml file.
- **2**: Create service using service manifest file using selector.

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: dev
    app: nginx
spec:
  containers:
    - name: nginxweb
      image: nginx:latest
      ports:
        - name: nginxwebport
          containerPort: 80
          protocol: TCP
```

Now Create a pod nginx from

```bash
kubectl create -f <filename>
```

check our pods using

```bash
kubectl get pod
```

Get the service using below command

```bash
kubectl get svc
```

==========================

```bash
apiVersion: v1
kind: Service
metadata:
  name: nginxservice
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

Create a service from

```bash
kubectl create -f s<filename>
```

Describe the service

```bash
kubectl describe service nginxservice
```

Get the service using below command

```bash
kubectl get svc
```

Retrieve the URL of a service running in a Minikube cluster

```bash
minikube service nginxservice --url

curl http://192.168.59.113:31050
```

delete service nginx

```bash
kubectl delete svc nginxservice

kubectl delete pod nginx
```
