> you should specify namespace before pod

```
kubectl exec --stdin --tty -n dj postgres-backend-7f5984df56-hc4sl -- /bin/bash
```

> you are able to install sudo inside pod and docker container
> 
```
apt-get install sudo
```
