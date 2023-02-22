# Kubernetes Labels

Labels in Kubernetes are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users but are not used by the Kubernetes itself. Labels are fundamental qualities of the object that will be used for grouping, viewing, and operating. Each object can have a set of key/value labels defined. Each Key must be unique for a given object.

- To label, a resource use the below command

  **Syntax**

```bash
kuectl label resource-type resource-name env=prod(label-name)
```

- Rename the label by using the below command

  **Syntax**

```bash
kuectl label --overwrite resource-type resource-name env=testing(label-name)
```

- Delete the label by using the below command

  **Syntax**

```bash
kuectl label resource-type resource-name env
```

- label all the pods in a namespace with the same label use the following command

  **Syntax**

```bash
kuectl label resource-type --all env=prod(label-name)
```
