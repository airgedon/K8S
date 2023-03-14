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

gcloud container clusters create --machine-type n1-standard-2 --num-nodes 2 --zone `us-central1` --cluster-version latest `autopilot-cluster-1`
