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

Install ingress controller

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-0.32.0/deploy/static/provider/kind/deploy.yaml
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
# Updated helm repo: https://artifacthub.io/packages/helm/oauth2-proxy/oauth2-proxy
#helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
#helm repo update
helm upgrade --install --wait --create-namespace --namespace tools oauth2-proxy oauth2-proxy/oauth2-proxy -f charts/oauth2-proxy/values.yaml
```

Add Ingress rule for the Tekton Dashboard

```shell
kubectl apply -n tekton-pipelines -f manifests/tekton/dashboard-ingress-oauth2.yaml
```

and hit [http://tekton-dashboard.127.0.0.1.nip.io/](http://tekton-dashboard.127.0.0.1.nip.io/) to be authenticated and logged in.

**NOTE:** logout url seems quirky to configure. Should be the possible through the deployment but somehow patching does not work.
