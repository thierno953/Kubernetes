# ConfigMap

- A ConfigMap is an API object used to store non-confidential data in key-value pairs.
- Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.
- ConfigMap does not provide secrery or encryption.
- The data stored in a ConfigMap cannot exceed 1 MiB.

## Different Use Cases For ConfigMap

- Environment variables for a container.
- As command and arguments inside container.
- Add a file in read-only volume.

```bash
kubectl get cm
```

- all namespaces

```bash
kubectl get cm --all-namespaces
```

- explain configMap

```bash
kubectl explain cm
```

**Example 1**

```bash
kubectl create configmap config-example-values --from-literal=city=Brussels --from-literal=country=Belgium

kubectl get cm

kubectl describe cm config-example-values

kubectl delete cm config-example-values
```

- **Example 2**

  **vi test-configmap.properties**

```shell
#!/bin/bash
city: Brussels
country: Belgium
```

```shell
kubectl create configmap test-configmap --from-file=./test-configmap.properties

kubectl get cm

kubectl delete cm test-configmap <NAME>
```

======================

**vi configmap.yml**

```shell
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-configmap
data:
  city: Brussels
  country: Belgium
```

- create a config map

```bash
kubectl create -f configmap.yml

kubectl get cm

kubectl describe cm test-configmap
```

**vi configmappod.yml**

```shell
---
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: configmap-container
      image: httpd
      envFrom:
        # Load the Complete ConfigMap
        - configMapRef:
            name: test-configmap

---
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: configmap-container
      image: httpd
      env:
        - name: CITY
          valueFrom:
            configMapKeyRef:
              name: test-configmap
              key: city
        - name: COUNTRY
          valueFrom:
            configMapKeyRef:
              name: test-configmap
              key: country
```

```bash
kubectl apply -f configmappod.yml

kubectl get pods

kubectl exec -it <NAME> bash

ls

printenv

exit

kubectl get pods

kubectl delete pod <NAME>
```
