# DaemonSet

- A DaemonSet ensures that all Nodes run a copy of a Pod.
- When nodes are added to the cluster, Pods are added to nodes.

  ```shell
   ---------------------------------------DaemonSet ----------------------------
   |                                        |                                  |
  Node1                                    Node2                              Node3
   |                                        |                                  |
  ------                                  -------                             -----
  pod                                      pod                                 pod
  ```

## Use Cases of DaemonSet

- Running a cluster storage daemon on every node.
- Running a logs collection daemon on every node.
- Running a logs monitoring daemon on every node.

```bash
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginxdaemonset
spec:
  selector:
    matchLabels:
      app: nginxdaemonset
  template:
    metadata:
      labels:
        app: nginxdaemonset
    spec:
      tolerations:
        - key: node-role.kubernetes.io/master
          effect: NoSchedule
          operator: Equal
          value: large
          tolerationSeconds: 60

      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

explain daemonset

```bash
kubectl explain daemonset
```

Now deploy the above daemonset using

```bash
kubectl apply -f daemonset.yml
```

get the daemonset using below command

```bash
kubectl get ds
```

check our pods using

```bash
kubectl get pods

kubectl get pods -o wide

kubectl get nodes
```
