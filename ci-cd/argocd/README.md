# Argo CD

See

- [argocd](https://argo-cd.readthedocs.io/en/stable/)
- [argocd-operator](https://operatorhub.io/operator/argocd-operator)

## Getting Started

Install the ArgoCD-CLI from [argo-cd/releases](https://github.com/argoproj/argo-cd/releases).

Next install ArgoCD into your Kubernetes cluster

```shell
$ kubectl create namespace argocd
namespace/argocd created

# Direct installation
# kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Recommended: Install using the ArgoCD operator, https://operatorhub.io/operator/argocd-operator
$ kubectl create -f https://operatorhub.io/install/argocd-operator.yaml
subscription.operators.coreos.com/my-argocd-operator created

# After install, watch your operator come up using next command.
$ kubectl get csv -n operators -w
NAME                      DISPLAY   VERSION   REPLACES                  PHASE
argocd-operator.v0.0.15   Argo CD   0.0.15    argocd-operator.v0.0.14   Installing
argocd-operator.v0.0.15   Argo CD   0.0.15    argocd-operator.v0.0.14   Succeeded

# Check the operator pod
λ kubectl get pods -n operators
NAME                               READY   STATUS    RESTARTS   AGE
argocd-operator-84756ccf88-6l2zt   1/1     Running   0          3m45s
```
