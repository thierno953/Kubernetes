# Delete Kubernetes Resources

- The simple method of deleting any resource in Kubernetes is to use the specific manifest file used to create it. With the manifest file on hand, we can use the kubectl delete command with the -f flag.
- **Syntax**

```bash
kubectl delete -f <path/to/file>
```

**Example**

```bash
kubectl delete -f namespace.yml
```

## Deleting Resources

A manifest file is not required for deleting resources. Instead, the resource can be targeted directly using the kubectl delete command. This method is more effective when targeting a group of resources or for deleting all resources in the cluster or a namespace.

**Syntax**

```bash
kubectl delete <type> <name> [-n <namespace>] | --all | -l <label>]
```

**Dry-Run**
Deleting resources can be a risky operation. When using complex matches for deleting resources it is a good practice to perform a dry-run of the deletion. This will allow you to preview exactly what Kubernetes will delete, ensuring you do not accidentally delete something in error.

```bash
kubectl delete ns --all --dry-run
```

## Deleting All Resources

- Current Namespace
  To do a mass delete of all resources in your current namespace context, you can execute the kubectl delete command with the -all flag.

```bash
kubectl delete --all
```

## Specific Namespace

To delete all resources from a specific namespace us the -n flag.

```bash
kubectl delete -n wordpress --all
```

## All Namespaces

To delete all resources from all namespaces we can use the -A flag.

```bash
kubectl delete -A
```

## Deleting Resource Types

- Deleting Namespaces
  **Syntax**

```bash
kubectl delete ns <name>
```

**Example**

```bash
kubectl delete ns myapp
```

## Deleting Pods

**Syntax**

```bash
kubectl delete pod <name> [-n <namespace>]
```

**Example #1**

```bash
kubectl delete pod worker-cgxxv
```

To delete a pod located in a different namespace you must use the -n flag with the namespace’s name. Example #2

```bash
kubectl delete pod worker-cgxxv -n myapp
```

## Deleting Services

**Syntax**

```bash
kubectl delete svc <name>
```

**Example 1**

```bash
kibectl delete svc nginx
```

**Example 2**

```bash
kubectl delete svc nginx -n wordpress
```

## Deleting Deployments

```bash
kubectl delete deployment <name>
```
