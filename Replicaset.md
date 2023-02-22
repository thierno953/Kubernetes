# ReplicaSet

- ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time.
- ReplicaSet guarantee the availability of a specified number of identical Pods.
- ReplicaSet will have the fields selector to identify the pods that it can select, replication count definning how many pods it sould be maintaining, pod template to specify what kind of pod it should create.

```bash
apiVersion: apps/v1
kind: ReplicaSet             # set resources
metadata:                    # replicaset-specific metadata
  name: nginxfrontend
spec:                        # conf to replicaset
  replicas: 3                # number of replicas
  selector:                  # use of selection
    matchLabels:             # selection on labels
      app: frontend
  template:                  # pod template features
    metadata:                # created pod metadata
      labels:                # definition of labels
        app: frontend        # creation of label that will match
    spec:                    # pod spec
      containers:
        - name: nginxserver
          image: nginx
          ports:
            - name: nginxwebport
              containerPort: 80
              protocol: TCP

```

explain replicaset

```bash
kubectl explain replicaset
```

Now deploy the above RS using

```bash
kubectl create -f replicaset.yml --save-config
```

check our pods using

```bash
kubectl get pods
```

get the replication controller using below command

```bash
kubectl get rs
```

Describe the ReplicationController:

```bash
kubectl describe rs/nginxfrontend
```

Let’s delete one pod and check if any new pods get created.

```bash
kubectl delete pod <podname>

kubectl get pods -o wide
```

To scale up the replicas:

**Example**

 change replicas: 3 --> replicas: 5
|-------------------------------------------------------------------------------------|

```bash
kubectl scale rs --replicas=5 nginxfrontend
```

Edit and replace replicas

```bash
kubectl edit rs nginxfrontend

kubectl replace -f replicaset.yml
```

check our pods using

```bash
kubectl get pods
```

get the replication controller using below command

```bash
kubectl get rs
```

To delete the ReplicationController:

```bash
kubectl delete pods --all

kubectl delete rs nginxfrontend
```
