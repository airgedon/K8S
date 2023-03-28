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

# Declarative Management and `kubectl apply`

### Rolling out a Deployment

> For example, to deploy the sample Nginx Deployment to your cluster, use apply and provide the path to the nginx-deployment.yaml manifest file:

```
kubectl apply -f nginx-deployment.yaml
```

> You can track the rollout status using rollout status:

```
kubectl rollout status deployment/nginx-deployment
```

> An alternative to rollout status is the kubectl get command, along with the -w (watch) flag:

```
kubectl get deployment -w
```

> Using rollout pause and rollout resume, you can pause and resume the rollout of a Deployment:

```
kubectl rollout pause deployment/nginx-deployment
```

```
kubectl rollout resume deployment/nginx-deployment
```

### Modifying a Running Deployment

###### If you’d like to modify a running Deployment, you can make changes to its manifest file and then run kubectl apply again to apply the update.

> The kubectl diff command allows you to see a diff between currently running resources, and the changes proposed in the supplied configuration file:

```
kubectl diff -f nginx-deployment.yaml
```

> Now allow Kubernetes to perform the update using apply:

```
kubectl apply -f nginx-deployment.yaml
```

###### If you run apply again without modifying the manifest file, Kubernetes will detect that no changes were made and won’t perform any action.

> Using rollout history you can see a list of the Deployment’s previous revisions:

```
kubectl rollout history deployment/nginx-deployment
```

> With rollout undo, you can revert a Deployment to any of its previous revisions:

```
kubectl rollout undo deployment/nginx-deployment --to-revision=1
```
### Deleting a Deployment

> To delete a running Deployment, use kubectl delete:

```
kubectl delete -f nginx-deployment.yaml
```

# Imperative Management

###### You can also use a set of imperative commands to directly manipulate and manage Kubernetes resources.

### Creating a Deployment


> Use create to create an object from a file, URL, or STDIN. Note that unlike apply, if an object with the same name already exists, the operation will fail.
 
 
>  The --dry-run flag allows you to preview the result of the operation without actually performing it:

```
kubectl create -f nginx-deployment.yaml --dry-run=client
```

> We can now create the object:

```
kubectl create -f nginx-deployment.yaml
```

### Modifying a Running Deployment

> Use scale to scale the number of replicas for the Deployment from 2 to 4:

```
kubectl scale --replicas=4 deployment/nginx-deployment
```

> You can edit any object in-place using kubectl edit. This will open up the object’s manifest in your default editor:

```
kubectl edit deployment/nginx-deployment
```

> Now run a get to inspect the changes:

```
kubectl get deployment/nginx-deployment
```

###### We’ve successfully scaled the Deployment back down to 2 replicas on-the-fly. You can update most of a Kubernetes’ object’s fields in a similar manner.

###### Another useful command for modifying objects in-place is kubectl patch. Using patch, you can update an object’s fields on-the-fly without having to open up your editor. patch also allows for more complex updates with various merging and patching strategies. 

[Update API Objects in Place Using kubectl patch](https://kubernetes.io/docs/tasks/run-application/update-api-object-kubectl-patch)

> The following command will patch the nginx-deployment object to update the replicas field from 2 to 4; deploy is shorthand for the deployment object.

```
kubectl patch deploy nginx-deployment -p '{"spec": {"replicas": 4}}'
```
> We can now inspect the changes:

```
kubectl get deployment/nginx-deployment
```

> You can also create a Deployment imperatively using the run command. run will create a Deployment using an image provided as a parameter:

```
kubectl run nginx-deployment --image=nginx --port=80 --replicas=2
```
> The expose command lets you quickly expose a running Deployment with a Kubernetes Service, allowing connections from outside your Kubernetes cluster:

```
kubectl expose deploy nginx-deployment --type=LoadBalancer --port=80 --name=nginx-svc
```

###### Here we’ve exposed the nginx-deployment Deployment as a LoadBalancer Service, opening up port 80 to external traffic and directing it to container port 80. We name the service nginx-svc.

> Using the LoadBalancer Service type, a cloud load balancer is automatically provisioned and configured by Kubernetes. To get the Service’s external IP address, use get:

```
kubectl get svc nginx-svc
```

> You can access the running Nginx containers by navigating to EXTERNAL-IP in your web browser.

## Inspecting Workloads and Debugging
