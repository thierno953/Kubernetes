# Secrets

- A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.
- Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.
- Pods can reference to the Secrets, as container environment variables or mounted volume to the container.

  ## Types of Kubernetes secret

When creating a Secret, you can specify its type using the type field of a Secret resource.

```bash
Builtin Type                              |                Usage
-----------------------------------------------------------------------------------------
Opaque                                    |             arbitrary user-defined data
Kubernetes.io/service-account-token       |             service account token
Kubernetes.io/dockercfg                   |             serialized ~/.dockercfg file
Kubernetes.io/dockerconfigjson            |             serialized ~/.docker/config.json file
Kubernetes.io/basic-auth                  |             credentials for basic authentication
Kubernetes.io/ssh-auth                    |             credentials for ssh authentication
Kubernetes.io/tls                         |             data for a TLS client or server
bootstrap.kubernetes.io/token             |             bootstrap token data
```

**secretstest.yml**

```shell
apiVersion: v1
kind: Secret
metadata:
  name: secret-test
type: kubernetes.io/basic-auth
stringData:
  username: admin
  password: pass123
```

```bash
kubectl get secrets --all-namespaces

kubectl apply -f secretstest.yml

kubectl get secrets

kubectl describe secret <NAME>

kubectl get secret <NAME>

kubectl get secret <NAME> -o yaml

echo cGFzczEyMw== | base64 -d ; echo

echo YWRtaW4= | base64 -d ; echo

echo -n "admin" | base64

echo -n "pass123" | base64

kubectl delete secret <NAME>
```

**secretnew.yml**

```shell
apiVersion: v1
kind: Secret
metadata:
  name: secret-test
type: kubernetes.io/basic-auth
data:
  username: YWRtaW4=
  password: cGFzczEyMw==
```

```bash
kubectl apply -f secretnew.yml

kubectl get secrets

kubectl get secret <NAME> -o yaml
```

### To inject multiple secret

**podsecret1.yml**

```shell
apiVersion: v1
kind: Pod
metadata:
  name: secret-test-env
spec:
  containers:
    - name: env-test-container
      image: nginx
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: secret-test
              key: username
```

### Inject secret as file

**podsecret.yml**

```shell
apiVersion: v1
kind: Pod
metadata:
  name: secret-test-pod
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
        # name must match the volume name below
        - name: secret-volume
          mountPath: /etc/secret-volume
  # The secret data is exposed to Containers in the Pod through a Volume.
  volumes:
    - name: secret-volume
      secret:
        secretName: secret-test
```

```bash
kubectl apply -f podsecret.yml

kubectl get pods

kubectl exec -it <NAME> -- /bin/bash

cd /etc/secret-volume

ls

cat password ; echo

cat username ; echo
```
