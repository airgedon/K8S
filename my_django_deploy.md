```
sudo nano django-deployment.yml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: _NAME_
  labels:
    app: django
spec:
  replicas: 3
  selector:
    matchLabels:
      app: django
  template:
    metadata:
      labels:
        app: django
    spec:
      containers:
        - image: deepmatr1x/django:k8s_first
          name: django
          ports:
            - containerPort: 8000
              name: gunicorn

```

```
kubectl apply -f django-deployment.yaml 
```

```
kubectl get deploy _NAME_
```

```
kubectl expose deploy _NAME_ --type=LoadBalancer --port=8000 --name=django-svc
```
```
kubectl get svc
```




