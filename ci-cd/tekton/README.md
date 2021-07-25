# Tekton

## Installation

### Tekton Operator

#### Managed

To install the tekton-operator and a via Operatorhub (requires OLM to be installed)

```shell
# Install operator subscription, https://operatorhub.io/operator/tektoncd-operator
$ kubectl create -f https://operatorhub.io/install/tektoncd-operator.yaml
subscription.operators.coreos.com/my-tektoncd-operator created

# Review subscription
$ kubectl get sub -n operators
NAME                   PACKAGE             SOURCE                  CHANNEL
my-tektoncd-operator   tektoncd-operator   operatorhubio-catalog   alpha

# Review the cluster service version (csv)
$ kubectl get csv -n operators
NAME                          DISPLAY             VERSION    REPLACES   PHASE
tektoncd-operator.v0.23.0-2   Tektoncd Operator   0.23.0-2              Succeeded

# Now, the operator should be running
$ kubectl get po -n operators
NAME                               READY   STATUS    RESTARTS   AGE
tekton-operator-77665df68c-v2tfm   1/1     Running   0          16m
```

Next, install [Tekton components using installation profile `all`](https://github.com/tektoncd/operator#install-tektoncd-operator):

```shell
$ kubectl create -f https://raw.githubusercontent.com/tektoncd/operator/main/config/crs/kubernetes/config/all/operator_v1alpha1_config_cr.yaml
tektonconfig.operator.tekton.dev/config created

# View the created TektonConfig
$ kubectl get tektonconfig.operator.tekton.dev/config -o jsonpath='{.metadata.name}{'\t'}{.spec}'
'config {"profile":"all","targetNamespace":"tekton-pipelines"}'

# Wait for tekton components to be ready
λ kubectl get deployments -n tekton-pipelines
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
tekton-dashboard                    1/1     1            1           12m
tekton-operator-proxy-webhook       1/1     1            1           13m
tekton-pipelines-controller         1/1     1            1           13m
tekton-pipelines-webhook            1/1     1            1           13m
tekton-triggers-controller          1/1     1            1           13m
tekton-triggers-core-interceptors   1/1     1            1           13m
tekton-triggers-webhook             1/1     1            1           13m
```

#### Manual

**NOTE:** Prefer Managed installation using OLM (see above).

[Install tekton-pipelines operator](https://github.com/tektoncd/operator#install-tektoncd-operator)

```shell
$ kubectl apply -f https://storage.googleapis.com/tekton-releases/operator/latest/release.yaml
namespace/tekton-operator created
...
```

This command automatically installs the latest official release of the Tekton core component, [Tekton Pipelines](https://github.com/tektoncd/pipeline), [Tekon Dashboard](https://github.com/tektoncd/dashboard), ...

### Tekton CLI

Next [install the Tekton CLI `tkn`](https://tekton.dev/docs/getting-started/#set-up-the-cli).

## Tasks

Create & start task

```shell
$ kubectl apply -f tasks\task-hello.yaml
task.tekton.dev/hello created

$ tkn task start hello [--dry-run]
TaskRun started: hello-run-f96m8

In order to track the TaskRun progress run:
tkn taskrun logs hello-run-f96m8 -f -n default

$ tkn taskrun logs --last -f
[hello] Hello World!
```

## Dashboard

Install ingress controller, see [NGINX Ingress Controller - Provider Specific Steps](https://kubernetes.github.io/ingress-nginx/deploy/#provider-specific-steps)

```shell
# Docker Desktop
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.47.0/deploy/static/provider/cloud/deploy.yaml

# minikube
minikube addons enable ingress

# microk8s
microk8s enable ingress

# kind
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

### Unsecured Ingress

Add ingress rule using [nip.io](https://nip.io)

```shell
kubectl apply -n tekton-pipelines -f manifests/tekton/dashboard-ingress.yaml
```

and hit [http://tekton-dashboard.127.0.0.1.nip.io/](http://tekton-dashboard.127.0.0.1.nip.io/)

### Secured Access via Oauth2

[Secure Tekton dashboard behind Oauth2-Proxy](https://github.com/tektoncd/dashboard/blob/main/docs/walkthrough/walkthrough-oauth2-proxy.md)

Register an OAuth Application (e.g. as [GitHub Developer App](https://github.com/settings/developers)).

Next, install oauth2-proxy using `CLIENT_ID / CLIENT_SECRET`

```shell
# For federated Connectors use dex: https://artifacthub.io/packages/helm/dex/dex
#helm repo add dex https://charts.dexidp.io
#helm repo update
helm upgrade -i --wait --create-namespace -n tools dex dex/dex -f charts/dex.values.yaml

# Updated helm repo: https://artifacthub.io/packages/helm/oauth2-proxy/oauth2-proxy
#helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
#helm repo update
helm upgrade -i --wait --create-namespace -n tools oauth2-proxy oauth2-proxy/oauth2-proxy -f charts/oauth2-proxy.values.yaml
```

Add Ingress rule for the Tekton Dashboard

```shell
kubectl apply -n tekton-pipelines -f ./manifests/tekton/dashboard-ingress-oauth2.yaml
```

and hit [http://tekton-dashboard.127.0.0.1.nip.io/](http://tekton-dashboard.127.0.0.1.nip.io/) to be authenticated and logged in.

**NOTE:** logout url seems quirky to configure. Should be the possible through the deployment but somehow patching does not work.

## Docker Registry

When building images we need a registry to push to, e.g.

- [twuni/docker-registry](https://artifacthub.io/packages/helm/twuni/docker-registry) (ArtifactHub), [twuni/docker-registry.helm](https://github.com/twuni/docker-registry.helm) (GitHub)
- [harbor](https://artifacthub.io/packages/helm/harbor/harbor)
- RedHat's Project [Quay](https://github.com/quay/quay-operator) (requires OpenShift)
- MiniKube: [minikube addons enable registry](https://minikube.sigs.k8s.io/docs/handbook/registry/)
- MicroK8s: [microk8s enable registry](https://microk8s.io/docs/registry-built-in)

### Plain Docker Registry

```shell
$ helm repo add twuni https://helm.twun.io
"twuni" has been added to your repositories

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "twuni" chart repository
...
Update Complete. ⎈Happy Helming!⎈

$ helm upgrade -i --wait --create-namespace -n tools my-docker-registry twuni/docker-registry -f ./charts/docker-registry.values.yaml
```

And check repositories at [http://registry.127.0.0.1.nip.io/v2/\_catalog](http://registry.127.0.0.1.nip.io/v2/_catalog).

### Harbor

```shell
helm repo add harbor https://helm.goharbor.io
helm  upgrade -i --wait --create-namespace -n tools my-harbor harbor/harbor --version 1.7.0  -f ./charts/harbor.values.yaml
```

And hit harbor at [harbor-core.127.0.0.1.nip.io](https://harbor-core.127.0.0.1.nip.io/) (`admin/Harbor12345`).

To push an image

```shell
docker login harbor-core.127.0.0.1.nip.io -u harbor_registry_user -p harbor_registry_password
```
