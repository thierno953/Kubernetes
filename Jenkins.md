# Setup Jenkins On Kubernetes Cluster

- Create a Namespace for Jenkins.

```bash
kubectl get namespaces

kubectl create namespace jenkins
```

- Create Jenkins deployment yml file
- Create Jenkins Service yml file.
- Make Jenkins accessible outside by exposing as a service.
- Access the Jenkins application on a Node Port.

**jenkinsdeployment.yml**

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins
          image: jenkins/jenkins:lts-jdk11
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: jenkins-home
              mountPath: /var/jenkins_home
      volumes:
        - name: jenkins-home
          emptyDir: {}
```

**jenkinsservice.yml**

```shell
apiVersion: v1
kind: Service
metadata:
  name: jenkins
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
  selector:
    app: jenkins
```

- If you don’t want the local storage persistent volume, you can replace the volume definition in the deployment with the host directory as shown below.

```bash
 volumes:
    - name: jenkins-home
      emptyDir: {}
```

- Create jenkins deployment yml file.

```bash
kubectl create -f jenkinsdeployment.yml -n jenkins
```

- Check the deployment status

```bash
kubectl get deployments -n jenkins

kubectl get pods -n jenkins

kubectl logs <NAME> -n jenkins
```

- Create the service using kubectl.

```bash
kubectl create -f jenkinsservice.yml -n jenkins
```

```bash
kubectl get services

kubectl get services -n jenkins

minikube ip

kubectl logs <NAME> -n jenkins
```
