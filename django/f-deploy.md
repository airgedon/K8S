> settings.py

```
DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
```

> Dockerfile

```dockerfile
FROM ubuntu:20.04

RUN apt-get update && apt-get install -y tzdata && apt install -y python3.8 python3-pip

RUN apt install python3-dev libpq-dev nginx -y

RUN pip install django gunicorn psycopg2

ENV PYTHONUNBUFFERED 1

ADD . ___WORKDIR___

WORKDIR ___WORKDIR___

COPY . .


RUN pip install -r requirements.txt

EXPOSE 8000

CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "__name__.wsgi"]

```

```
docker build -t djangokubernetesproject.
```

```
docker run -i -t djangokubernetesproject sh
```

```
python3 manage.py makemigrations && python3 manage.py migrate
```
```
python3 manage.py collectstatic
```
```
python3 manage.py createsuperuser
```
> check if running

> authorize to Docker Hub

```
docker login
```
```
docker tag [image id] deepmatr1x/django:tagname
```
```
docker push deepmatr1x/django:tagname
```
---

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
      app: __NAME__
  template:
    metadata:
      labels:
        app: django
    spec:
      containers:
        - image: __IMAGE_NAME__
          name: django
          ports:
            - containerPort: __POST__
              name: gunicorn

```

```
kubectl apply -f django-deployment.yaml 
```

```
kubectl get deploy _NAME_
```

```
kubectl expose deploy _NAME_ --type=LoadBalancer --port=__POST__ --name=__NAME__
```
```
kubectl get svc
```




