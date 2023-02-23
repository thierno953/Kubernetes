# Deploying Node.js Application In Kubernetes

- Create a Node.js Application
- Create a Dockerfile
- Build your image
- Push your image to Docker Hub
- Write your kubernetes deployment yml
- Write your kubernetes service yml
- Deploy your application in kubernetes

**vi Dockerfile**

```shell
FROM node:latest

WORKDIR /usr/src/app

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

docker build and push image to docker hub

```bash
docker build -t thiernos/nodeapp:latest .

docker images

docker run -d -p 3000:3000 thiernos/nodeapp

docker ps

docker rm -f <ID>

docker login

docker push thiernos/nodeapp:latest
```

- Deployment Service

**vi deploymentservice.yml**

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodeapp-deployment
  labels:
    app: nodeapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nodeapp
  template:
    metadata:
      labels:
        app: nodeapp
    spec:
      containers:
      - name: nodeserver
        image: thiernos/nodeapp:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "250m"
        ports:
        - containerPort: 3000


---

apiVersion: v1
kind: Service
metadata:
  name: nodeapp-service
spec:
  selector:
    app: nodeapp
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 3000
    nodePort: 31110
```

- Write Kubernetes deployment yml

**vi deployment.yml**

```shell
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodeapp-deployment
  labels:
    app: nodeapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nodeapp
  template:
    metadata:
      labels:
        app: nodeapp
    spec:
      containers:
        - name: nodeserver
          image: thiernos/nodeapp:latest
          ports:
          - containerPort: 3000
```

```bash
kubectl get svc

kubectl apply -f deployment.yml

kubectl get deployment

kubectl get pods
```

- write Kubernetes service yml

**vi service.yml**

```shell
apiVersion: v1
kind: Service
metadata:
  name: nodeapp-service
spec:
  selector:
    app: nodeapp
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 3000
    nodePort: 31110
```

```bash
kubectl apply -f service.yml

kubectl get svc

minikube service nodeapp-service
```
