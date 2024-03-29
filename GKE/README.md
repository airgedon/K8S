## Install  Gcloud SDK
> Download and install the gcloud command line tool. It will help you create and communicate with a Kubernetes cluster.

[.](https://z2jh.jupyter.org/en/latest/kubernetes/google/step-zero-gcp.html)

```
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-421.0.0-linux-x86_64.tar.gz
```

###### To replace an existing installation, remove the existing google-cloud-sdk directory and then extract the archive to the same location. 
 
```
tar -xf google-cloud-cli-421.0.0-linux-x86_64.tar.gz 
```
```
./google-cloud-sdk/install.sh
```
###### Modify profile to update your $PATH and enable bash completion? (Y/n)   `Y`

> :warning: Close the terminal

---

>  To initialize the gcloud CLI, run gcloud init:

```
./google-cloud-sdk/bin/gcloud init
```
##### Activate API one time in the beginning
```
gcloud services enable container.googleapis.com
```

---
## Install kubectl

```
snap install kubectl --classic
```

```
kubectl version --client
```
---

### You will need to install the gke-gcloud-auth-plugin binary on all systems where kubectl or Kubernetes custom clients are used. To install the binary, use one of the following methods.

> Install using "gcloud components install"

```
gcloud components install gke-gcloud-auth-plugin
```
---


> Connect to the cluster that have already been created in cloud
```
gcloud container clusters get-credentials my-first-cluster-1 --zone us-central1-c --project deep-matrix
```
⚠️: use zonal clusters not regional because it needs more quotes 300(meanwhile you only have 250 limit)


