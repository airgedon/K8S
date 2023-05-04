# To create a TLS secret using certbot, you can follow these steps:

- Install certbot on your local machine or server. You can find installation instructions for various operating systems on the certbot website.



- Run certbot to obtain an SSL certificate for your domain. You can use the following command:

```
certbot certonly --manual --preferred-challenges=dns -d deep-matrix.site -d *.deep-matrix.site
```
> Replace example.com with your domain name. This command will prompt you to create a DNS TXT record to verify that you own the domain. Follow the instructions provided by certbot to create the DNS record.

> Once certbot has verified that you own the domain, it will generate a private key and SSL certificate for you. The private key will be stored in /etc/letsencrypt/live/example.com/privkey.pem, and the SSL certificate will be stored in /etc/letsencrypt/live/example.com/fullchain.pem.

- Create a Kubernetes TLS secret using the following command:

```
kubectl create secret tls letsencrypt-staging --key=/etc/letsencrypt/live/deep-matrix.site/privkey.pem --cert=/etc/letsencrypt/live/deep-matrix.site/fullchain.pem
```

> Replace letsencrypt-staging with the name of your secret, and example.com with your domain name.


> note: value in output

- In your Kubernetes manifests, reference the letsencrypt-staging secret in the tls.secretName field of your Ingress resource and Certificate resource.

For example:
