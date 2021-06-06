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

Install ingress controller

```shell
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

## Docker Registry (Quay)

When building images with tekton & [kaniko](https://github.com/GoogleContainerTools/kaniko) we will need an image registry,
e.g RedHat's Project [Quay](https://github.com/quay/quay-operator):

```shell
# Install quay-operator: https://operatorhub.io/operator/project-quay (too old)
#$ kubectl create -f https://operatorhub.io/install/project-quay.yaml
#namespace/my-project-quay created
#operatorgroup.operators.coreos.com/operatorgroup created
#subscription.operators.coreos.com/my-project-quay created

# Review operator subscription is ok
#$ kubectl get sub -n my-project-quay
#NAME              PACKAGE        SOURCE                  CHANNEL
#my-project-quay   project-quay   operatorhubio-catalog   stable

#--------------
# Add the openshift-marketplace catalog source: https://github.com/quay/quay-operator#getting-started
$ kubectl create -f https://raw.githubusercontent.com/quay/quay-operator/master/bundle/quay-operator.catalogsource.yaml
catalogsource.operators.coreos.com/quay-operator-tng created

# Use operator from github
# Install minimal quay registry:

```
