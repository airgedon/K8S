# To create a TLS secret using certbot, you can follow these steps:

- Install certbot on your local machine or server. You can find installation instructions for various operating systems on the certbot website.



- Run certbot to obtain an SSL certificate for your domain. You can use the following command:

```
certbot certonly --manual --preferred-challenges=dns -d deep-matrix.site -d *.deep-matrix.site
```
> Replace example.com with your domain name. This command will prompt you to create a DNS TXT record to verify that you own the domain. Follow the instructions provided by certbot to create the DNS record.

> Once certbot has verified that you own the domain, it will generate a private key and SSL certificate for you. The private key will be stored in /etc/letsencrypt/live/example.com/privkey.pem, and the SSL certificate will be stored in /etc/letsencrypt/live/example.com/fullchain.pem.

```
kubectl create ns ev
```

- Create a Kubernetes TLS secret using the following command:


```
kubectl create secret tls eventerro-tls --key=/etc/letsencrypt/live/deep-matrix.site/privkey.pem --cert=/etc/letsencrypt/live/deep-matrix.site/fullchain.pem -n ev
```

> Replace letsencrypt-staging with the name of your secret, and example.com with your domain name.


> note: value in output

- In your Kubernetes manifests, reference the letsencrypt-staging secret in the tls.secretName field of your Ingress resource and Certificate resource.

For example:

```yml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    email: dozzylind@example.com
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
      - dns01:
          cloudDNS:
            project: deep-matrix
            serviceAccountSecretRef:
              name: teodor@deep-matrix.iam.gserviceaccount.com
              key: key.json

```

```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.crds.yaml -n ev
```

```
kubectl apply -f clusterIssuer.yaml -n ev
```

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: eventerro-tls
spec:
  dnsNames:
    - deep-matrix.site
  secretName: eventerro-tls
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

```
kubectl apply -f certificate.yaml -n ev
```

Note that in the above example, the tls.secretName field of the Ingress resource and the spec.secretName field of the Certificate resource are both set to letsencrypt-staging, which is the name of the Kubernetes secret that contains the SSL certificate and private key.

Additionally, the dnsNames field of the Certificate resource should be set to the domain names that the certificate is valid for (in this case, example.com). The issuerRef field should reference the name of the ClusterIssuer resource that you created earlier.

---

```yml
apiVersion: v1
kind: Service
metadata:
  name: eventerro
spec:
  type: LoadBalancer
  loadBalancerIP: 104.198.73.168
  selector:
    app: eventerro
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 80
  - name: https
    protocol: TCP
    port: 443
    targetPort: 443
  externalTrafficPolicy: Local
```
> ⚠️ create static External IP with premium and regional ( cause there are less restrictions) credentials in GCP

```
kubectl apply -f service.yaml -n ev
```

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: eventerro-deployment
spec:
  selector:
    matchLabels:
      app: eventerro
  replicas: 1
  template:
    metadata:
      labels:
        app: eventerro
    spec:
      containers:
      - name: eventerro
        image: deepmatr1x/react:eventerro
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: eventerro
  namespace: ev
spec:
  selector:
    app: eventerro
    app.kubernetes.io/component: controller
    app.kubernetes.io/name: ingress-nginx
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
    - name: https
      protocol: TCP
      port: 443
      targetPort: 443
  type: LoadBalancer
  externalTrafficPolicy: Local
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: eventerro
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-staging"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
    - hosts:
        - deep-matrix.site
      secretName: eventerro-tls
  rules:
    - host: deep-matrix.site
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: eventerro
                port:
                  name: https

```

```
kubectl apply -f eventerro.yaml -n ev
```
---

> Check

```
kubectl describe ingress -n ev
```

```
kubectl describe service -n ev
```

```
kubectl describe pod -n ev
```

```
kubectl logs -n ns eventerro-deployment-56c846d94f-prpw6
```

