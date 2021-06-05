# Tekton

## Installation

### Tekton Operator

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

### Access

```shell
$ kubectl -n tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
...
```

### Oauth2

[Secure Tekton dashboard behind Oauth2-Proxy](https://github.com/tektoncd/dashboard/blob/main/docs/walkthrough/walkthrough-oauth2-proxy.md)

Install ingress controller

```shell
# Install nginx-ingress (helm)
# helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
# helm repo update
#helm upgrade --install --wait --create-namespace --namespace ingress-nginx ingress-nginx ingress-nginx/ingress-nginx -f charts/ingress-nginx/values.yaml

# On MiniKube
# $ minikube addons enable ingress
#   - Using image k8s.gcr.io/ingress-nginx/controller:v0.44.0
#   - Using image docker.io/jettech/kube-webhook-certgen:v1.5.1
#   - Using image docker.io/jettech/kube-webhook-certgen:v1.5.1
# * Verifying ingress addon...
# * The 'ingress' addon is enabled

# Bare kubctl manifest, see
# - https://github.com/tektoncd/dashboard/blob/main/docs/walkthrough/walkthrough-kind.md#installing-nginx-ingress-controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-0.32.0/deploy/static/provider/kind/deploy.yaml
```

Register an OAuth Application (e.g. as [GitHub Developer App](https://github.com/settings/developers)).

Next, install oauth2-proxy using `CLIENT_ID / CLIENT_SECRET`

```shell
# Updated helm repo: https://artifacthub.io/packages/helm/oauth2-proxy/oauth2-proxy
#helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
#helm repo update

# # CLIENT_ID and CLIENT_SECRET are the Client ID and Client Secret obtained
# when creating the GitHub OAuth application in the previous step
CLIENT_ID=__THE_CLIENT_ID_OF_YOUR_GITHUB_OAUTH_APP__
CLIENT_SECRET=__THE_CLIENT_SECRET_OF_YOUR_GITHUB_OAUTH_APP__

helm upgrade --install --wait --create-namespace --namespace tools oauth2-proxy oauth2-proxy/oauth2-proxy -f charts/oauth2-proxy/values.yaml
```

Add Ingress rule for the Tekton Dashboard

```shell
kubectl apply -n tekton-pipelines -f manifests/tekton/dashboard-ingress.yaml
```

and hit [http://tekton-dashboard.127.0.0.1.nip.io/](http://tekton-dashboard.127.0.0.1.nip.io/) to be authenticated and logged in
