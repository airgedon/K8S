> Dockerfile

```
FROM ubuntu:20.04

RUN apt-get update && apt-get install -y tzdata && apt install -y python3.8 python3-pip

RUN apt install python3-dev libpq-dev nginx -y

RUN pip install django gunicorn psycopg2

ENV PYTHONUNBUFFERED 1

ADD . /home/ubuntu/Backend

WORKDIR /home/ubuntu/Backend

COPY . .


RUN pip install -r requirements.txt

EXPOSE 8000

CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "devsearch.wsgi"]

```


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




