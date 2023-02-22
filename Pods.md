# POD

- A pod contains the environment that put me to run one or more containers.

- A pod consists of a network environment of the storage environment and hendrix containers.

- **Smallest Execution Unit**
- **Network Space**
- **Volume Space**
- **Data**

## Nginx

```bash
apiVersion: v1                # Kubernetes API version
kind: Pod                     # Type of Kubernetes resource
metadata:                     # Metadata about the Pod
  name: nginx
  labels:
    env: dev
    app: nginx
spec:                         # The spec/blueprint for the pod
  containers:                 # Information about the containers that will run in the Pod
    - name: nginxweb
      image: nginx:latest
      ports:
        - name: nginxwebport
          containerPort: 80
          protocol: TCP
```

- Displays master and service URLs

```bash
kubectl cluster-info
```

- Command that provides detailed information about the fields and resources in Kubernetes:

```bash
kubectl explain pod
```

- Create a Pod from

```bash
kubectl create -f <PodName>
```

- Start this Pod by running:

```bash
kubectl apply -f <PodName>
```

- Check on its status with:

```bash
kubectl get pods
```

- Get an interactive shell from a Pod

```bash
kubectl exec -it nginx bash

cd /usr/share/nginx/html

cat index.html
```

- Edit pod configuration file

```bash
kubectl edit -f <PodName>
```

- Get information about a running pod

```bash
kubectl describe pod <PodName>
```

To list wide o/p of pod use below command

```bash
kubectl get pod nginx -o wide
```

To get the o/p in YAML format use the below command

```bash
kubectl get pod nginx -o yaml
```

To get the o/p in JSON format use the below command

```bash
kubectl get pods -o json
```

## Apache

```bash
apiVersion: v1
kind: Pod
metadata:
  name: apache
spec:
  containers:
    - name: httpdserver
      image: httpd
```
