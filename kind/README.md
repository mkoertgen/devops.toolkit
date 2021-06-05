# Kind

Works better with nip.io than e.g. minikube (especially on Windows)

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
