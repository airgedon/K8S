```
kubectl get nodes
```

```
kubectl cluster-info
```

```
kubectl get componentstatuses
```
---
### Configuring a service account and storing its credentials

> This procedure demonstrates how to create the service account for your GKE integration. It explains how to create the account, add roles to it, retrieve its keys, and store them as a base64-encoded encrypted repository secret named GKE_SA_KEY.

> in [console cloud](https://console.cloud.google.com/) get your Project ID

> Create a new service account:

```
gcloud iam service-accounts create __NAME__
```

> Retrieve the email address of the service account you just created:


```
gcloud iam service-accounts list
```

> Add roles to the service account
```
gcloud projects add-iam-policy-binding eco-spirit-380605 --member='serviceAccount:teodor@eco-spirit-380605.iam.gserviceaccount.com'  --role=roles/container.admin
```
```
gcloud projects add-iam-policy-binding eco-spirit-380605 --member='serviceAccount:teodor@eco-spirit-380605.iam.gserviceaccount.com'  --role=roles/storage.admin
```
```
gcloud projects add-iam-policy-binding eco-spirit-380605 --member='serviceAccount:teodor@eco-spirit-380605.iam.gserviceaccount.com'  --role=roles/storage.admin
```
