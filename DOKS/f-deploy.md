```
sudo ufw enbale
sudo ufw allow 22
```

```
sudo snap install kubectl --classic
```
```
sudo snap install doctl
```

> generate auth token in [API Tokens](https://cloud.digitalocean.com/account/api/tokens?i=d6c588)

```
doctl kubernetes cluster kubeconfig save use_your_cluster_name
```
> to save credentials file

```
doctl kubernetes cluster kubeconfig save <cluster-name>
```
###### done 
---
