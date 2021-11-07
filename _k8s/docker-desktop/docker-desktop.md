# Docker Desktop

On Windows, when already using Docker Desktop, the easiest option is probably to just [enable Kubernetes](https://docs.docker.com/desktop/kubernetes/).
In case you want excercise the full development lifecycle including building a container image and deploy to your local cluster you will need some extras.

## Ingress

Adding an Ingress Controller according to documentation, see the "nginx Installation Guide"

- [Docker Desktop](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)
- [Quick start](https://kubernetes.github.io/ingress-nginx/deploy/#quick-start)

```shell
$ helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
Release "ingress-nginx" does not exist. Installing it now.
NAME: ingress-nginx
LAST DEPLOYED: Sun Nov  7 15:45:26 2021
NAMESPACE: ingress-nginx
STATUS: deployed
...

# Check external ip `localhost` has been assigned to default LoadBalancer
$ kubectl -n ingress-nginx get svc ingress-nginx-controller
NAME                       TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller   LoadBalancer   10.106.104.101   localhost     80:32692/TCP,443:30901/TCP   95s

# Check default 404-page of nginx
$ curl http://some-host.localhost:80
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

## Container registry
