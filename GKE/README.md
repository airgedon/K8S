[Source Doc](https://z2jh.jupyter.org/en/latest/kubernetes/google/step-zero-gcp.html)

> :bulb: Download and install the gcloud command line tool. It will help you create and communicate with a Kubernetes cluster.

in your  home directory

```
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-421.0.0-linux-x86_64.tar.gz
```

> To replace an existing installation, remove the existing google-cloud-sdk directory and then extract the archive to the same location. 
 
```
tar -xf google-cloud-cli-421.0.0-linux-x86_64.tar.gz 
```
`N` -> `Y`

> Open a new terminal so that the changes take effect.

> :bulb: To initialize the gcloud CLI, run gcloud init:

```
./google-cloud-sdk/bin/gcloud init
```
---
> Install kubectl (reads kube control), it is a tool for controlling Kubernetes clusters in general. From your terminal, enter:

```
gcloud components install kubectl
```
> after creating k8s kluster

> CONNECT 
gcloud container clusters get-credentials autopilot-cluster-1 --region us-central1 --project eco-spirit-380605

> To test if your cluster is initialized, run:

```
kubectl get node

```
> The response should list two running nodes (or however many nodes you set with --num-nodes above).

---

 > Give your account permissions to perform all administrative actions needed.
 
```
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=dozzylind@gmail.com  
```

Did you enter your email correctly? If not, you can run `kubectl delete clusterrolebinding cluster-admin-binding` and do it again.

---

> Install the package manager for k8s for installing, upgrading and managing applications on a Kubernetes cluster ( Helm packages are called charts )

```
curl https://raw.githubusercontent.com/helm/helm/HEAD/scripts/get-helm-3 | bash
```
---

```
gcloud services enable container.googleapis.com
```
