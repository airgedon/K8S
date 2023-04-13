# Basic Components

- **Node** 
  - simple server
- **Pod**   
  -  abstraction over container 
  -  creates env on top of the container
  -  smallest unit of k8s
- **Service** and **Ingress**
  - each pod gets its own IP address ( they are able to communicate with each other using this internal IP not public )
  - when pod dies, new IP will resign to that pod
  - `Service` is the permanent IP and load balancer which means that service will actually catch the request and forward it to which pod list busy 
  - lifecycle of Pod and Service NOT connected
  - Ingress allow to use external service with (secures) domain name 
- **ConfigMap** and **Secret**
  - ConfigMap is the place where you can store your configuration data like database url
  - but you shouldn't put credentials into ConfigMap , you should store them in Secret
  - Secret is like ConfigMap but it used to store secret data
- **Volumes**
  - Volume component is used to store your db, it could be remote or local
  - cause when db pod dies, it's db would be gone , Volumes can help to avoid it (it is like external hard drive)
-  **Deployment** and **Stateful Set**
   - so when pod dies, there will be downtime where user can reach, so thats why we are replicating everything
   - we should specify how many replicas should run , define that blueprints and these are Deployment
   - you mostly work with Deployment rather than with nodes
   - if your pod dies, service will forward the request to another working replica to avoid downtime 
   - :warning: DB can't be replicated via Deployment
   - If we the clones replicas of db they would all need to  access the same shared data storage and there
     you would need mechanism that would manage which pods are reading from that storage in order to avoid data inconsistency 
     so here you need Stateful Set component

### K8s Architecture
#### 3 Node proccessses:
- **Kubelet** - proccess of k8s itself, that has interface both - the container and node.
   - it is responsible for taking that configuration and running the pod
- **Kube Proxy** - forwards the requests 
-  **Container Runtime** - in order to for k8s cluster function properly

#### So, how do you interact with cluster?
  
- How to:
 - schedule pod ?
 - monitor ?
 - re-schedule/r-start pod ?
 - join a new Node ?

> 🔑 Answer: all this managing proccesses are done by **Master Nodes**

# Namespace

#### Namespaces in Kubernetes allow objects to be grouped. Namespaces can be used for logical groupings

- By default, there are three namespaces in a K8s cluster, default, kube-public and kube-system.

```
a group of developers may only have access to a namespace called dev , and have no access to the production namespace.
```

```
The ability to limit different teams to different namespaces can be valuable to avoid duplicated work or resource conflict.
```

> Note the short format for namespace can be used (-n)

```
 kubectl get pods -n ingress-nginx
```
> To view pods in the default namespace (no namespace specified):

```
kubectl get pods
```
> To create a new namespace:

```
kubectl create namespace jacks-blog
```
⚠️ Names cannot start with a hyphen, and the alpha characters can only be lowercase.

> And to delete a namespace:

```
kubectl delete namespace jacks-blog
```
> To see details about a namespace:

```
kubectl describe namespace nginx-ingress
```

>  Show resource usage of pods in a namespace.

To see the resource usage (CPU / Memory) of pods in a particular namespace:

```
kubectl top pod --namespace=ingress-nginx
```
#### Creating a Namespace Using a YAML Configuration File

```
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: jacks-blog
```

```
kubectl apply -f namespace.yml
```

# How to Restart Kubernetes Pods With Kubectl
