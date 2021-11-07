# MiniKube

There are some Pros using [MiniKube](<(https://minikube.sigs.k8s.io/docs/start/)>) over Docker Desktop, see [Free Docker Desktop Alternative For Mac And Windows](https://www.youtube.com/watch?v=LGNEG-t96eE) (YouTube).

For local development you would be in favor of the following addons

- [Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/) using Local Storage,
- a [Container Registry](https://www.redhat.com/en/topics/cloud-native-apps/what-is-a-container-registry) and
- an [Ingress Controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).

For this, the most suitable solution seems to be .

## Create a local MiniKube Cluster

```shell
# Reset your Minikube cluster
$ minikube delete
* Stopping node "minikube"  ...
...
* Removed all traces of the "minikube" cluster.

# Create a MiniKube cluster using a specific (the latest) Kubernetes version and some addons
$ minikube start --kubernetes-version=v1.22.2 --addons ingress --addons registry
* minikube v1.23.2 auf Microsoft Windows 10 Pro 10.0.19043 Build 19043
...
* Verifying ingress addon...
* Verifying registry addon...
* Enabled addons: storage-provisioner, default-storageclass, registry, ingress
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

# You can also enable addons later
$ minikube addons list
|-----------------------------|----------|--------------|-----------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |      MAINTAINER       |
|-----------------------------|----------|--------------|-----------------------|
...
| default-storageclass        | minikube | enabled ✅   | kubernetes            |
| ingress                     | minikube | enabled ✅   | unknown (third-party) |
| registry                    | minikube | enabled ✅   | google                |
| storage-provisioner         | minikube | enabled ✅   | kubernetes            |
...
|-----------------------------|----------|--------------|-----------------------|
```

## Have your Docker client point to MiniKube

```shell
# Let MiniKube expose the necessary environment variables
$ eval $(minikube.exe docker-env)

# Check Docker CLient communicates with MiniKube's Docker daemon
$ docker container ls
CONTAINER ID   IMAGE                                          COMMAND                  CREATED          STATUS
841d75f4e126   k8s.gcr.io/ingress-nginx/controller            "/usr/bin/dumb-init …"   10 minutes ago   Up 10 minutes
4ddd97a01919   registry                                       "/entrypoint.sh /etc…"   10 minutes ago   Up 10 minutes
...
```

## Connect to the registry

See [](https://minikube.sigs.k8s.io/docs/handbook/registry/#docker-on-windows)

```shell
$ kubectl get service --namespace kube-system
NAME       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                  AGE
kube-dns   ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   17m
registry   ClusterIP   10.101.245.144   <none>        80/TCP,443/TCP           16m
```
