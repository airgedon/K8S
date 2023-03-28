### To test that kubectl can authenticate with and access your Kubernetes cluster

```
kubectl cluster-info
```
###### kubectl is configured using kubeconfig configuration files

### Modifying your kubectl Configuration

###### You can also modify your config using the `kubectl config` set of commands

> To view your kubectl configuration

```
kubectl config view
```
```
sudo nano .kube/config
```

### Modifying Clusters

> To fetch a list of clusters defined in your kubeconfig

```
kubectl config get-clusters
```

> To add a cluster to your config, use the `set-cluster` subcommand:

```
kubectl config set-cluster new_cluster --server=server_address --certificate-authority=path_to_certificate_authority
```
> To delete a cluster from your config

```
kubectl config delete-cluster
```

⚠️ This only deletes the cluster from your config and does not delete the actual Kubernetes cluster.

---

### Modifying Users

> You can perform similar operations for users using `set-credentials`:

```
kubectl config set-credentials username --client-certificate=/path/to/cert/file --client-key=/path/to/key/file
```
> To delete a user from your config, you can run unset:

```
kubectl config unset users.username
```
### Contexts

> A context in Kubernetes is an object that contains a set of access parameters for your cluster. It consists of a cluster, namespace, and user triple. Contexts allow you to quickly switch between different sets of cluster configuration.

> To see your current context, you can use current-context:

```
kubectl config current-context
```
> To see a list of all configured contexts, run get-contexts:

```
kubectl config get-contexts
```

> To set a context, use set-context:

```
kubectl config set-context context_name --cluster=cluster_name --user=user_name --namespace=namespace
```


>You can switch between contexts with use-context:

```
kubectl config use-context context_name
```

> And you can delete a context with delete-context:

```
kubectl config delete-context context_name
```

## Using Namespaces


> A Namespace in Kubernetes is an abstraction that allows you to subdivide your cluster into multiple virtual clusters. By using Namespaces you can divide cluster resources among multiple teams and scope objects appropriately. For example, you can have a prod Namespace for production workloads, and a dev Namespace for development and test workloads.

> To fetch and print a list of all the Namespaces in your cluster, use get namespace:

```
kubectl get namespace
```

> To set a Namespace for your current context, use set-context --current:

```
kubectl config set-context --current --namespace=namespace_name
```

> To create a Namespace, use create namespace:

```
kubectl create namespace namespace_name
```

> Similarly, to delete a Namespace, use delete namespace:

⚠️ Deleting a Namespace will delete everything in the Namespace, including running Deployments, Pods, and other workloads. Only run this command if you’re sure you’d like to kill whatever’s running in the Namespace or if you’re deleting an empty Namespace.

```
kubectl delete namespace namespace_name
```

> To fetch all Pods in a given Namespace or to perform other operations on resources in a given Namespace, make sure to include the --namespace flag:

```
kubectl get pods --namespace=namespace_name
```

## Managing Kubernetes Resources


> The general syntax for most kubectl management commands is:

```
kubectl command type name flags
```
Where

- command is an operation you’d like to perform, like create
- type is the Kubernetes resource type, like deployment
- name is the resource’s name, like app_frontend
- flags are any optional flags you’d like to include

> For example the following command retrieves information about a Deployment named app_frontend:

```
kubectl get deployment app_frontend
```

## Declarative Management and `kubectl apply`


 
