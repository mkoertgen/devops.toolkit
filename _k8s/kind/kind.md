# Kind

[Kind](https://kind.sigs.k8s.io/) works better with [nip.io](https://nip.io/) than e.g. [minikube](https://minikube.sigs.k8s.io/), especially on [Docker Desktop (Windows)](https://docs.docker.com/desktop/).

## Create a cluster

```shell
# Install Docker Desktop & Kind
#choco install docker-desktop kind -y

# Create a kind cluster
$ kind create cluster --name walkthrough --config kind.yaml
Creating cluster "walkthrough" ...
 • Ensuring node image (kindest/node:v1.21.1) �  ...
 ✓ Ensuring node image (kindest/node:v1.21.1) �
 • Preparing nodes �   ...
 ✓ Preparing nodes �
 • Writing configuration �  ...
 ✓ Writing configuration �
 • Starting control-plane �️  ...
 ✓ Starting control-plane �️
 • Installing CNI �  ...
 ✓ Installing CNI �
 • Installing StorageClass �  ...
 ✓ Installing StorageClass �
Set kubectl context to "kind-walkthrough"
You can now use your cluster with:

kubectl cluster-info --context kind-walkthrough

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community �
```

Install ingress controller

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

## Stop / Restart Cluster

Stopping / restarting a local Kind cluster is [not supported](https://github.com/kubernetes-sigs/kind/issues/831).
Yet as a workaround you can just pause & resume the respective container

```shell
# Get Docker container name of Kind
$ docker ps | grep kindest
528650083cbc   kindest/node:v1.21.1 ...
# Stop kind
$ docker pause 528650083cbc
528650083cbc
# Resume again
$ docker unpause 528650083cbc
```

## Cleanup

To [clean everything up](https://github.com/tektoncd/dashboard/blob/main/docs/walkthrough/walkthrough-kind.md#cleaning-up), run the following command:

```shell
kind delete cluster --name walkthrough
```
