# What is Kubernetes

- Kubernetes is a popular container orchestration tool similar to docker swarm, K8 is mostly used to manage the containers, it is also used for blue-green deployments, also use to scale the containers.

## Kubernetes architecture overview

- K8 nodes are divided into 2 types, master node(control plane), and worker node
- These nodes can be a physical machines as well as the virtual machines
- Master and Worker nodes have different components resided inside it

## Master Node

- Master is responsible for managing the complete cluster.
- You can access master node via the CLI, GUI, or API.
- The master watches over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.
- For achieving fault tolerance, there can be more than one master node in the cluster.
- It is the access point from which administrators and other users interact with the cluster to manage the scheduling and deployment of containers.
- It has four components: ETCD, Scheduler, Controller and API Server

## Master Node (Control Plane) components

- API Server
- ETCD
- Control Manager
- Scheduler
- Worker Node Components
- Kubelet
- kube-proxy

## What is Kubectl

- kubectl is the command line utility using which we can interact with k8s cluster
- Uses APIs provided by API server to interact.
- Also known as the kube command line tool or kubectl or kube control.
- Used to deploy and manage applications on a Kubernetes

## Minikube

- Minikube is set up within a single VM and it is very useful if you have limited resources, Minikube is not used for production environment setup, it is mostly used by the developers or to set up an environment for learning.

## install kubernetes

```bash
sudo apt-get update -y && sudo apt-get upgrade -y

sudo apt-get install curl

sudo apt-get install apt-transport-https

sudo apt-get install virtualbox virtualbox-ext-pack -y

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client

kubectl version --client --output=yaml
```

## install minikube

```bash
nproc

df -h

free -h

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube version

minikube start --driver=virtualbox

minikube status

kubectl get nodes
```
