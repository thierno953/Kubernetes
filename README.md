# Kubernetes

kind-config.yaml

```sh
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

```sh
kind create cluster --name kind-cluster --config kind-config.yaml

kind delete cluster -n kind-cluster
```
