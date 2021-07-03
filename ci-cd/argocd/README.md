# Argo CD

See

- [argocd](https://argo-cd.readthedocs.io/en/stable/)
- [argocd-operator](https://operatorhub.io/operator/argocd-operator)

## Getting Started

Install the ArgoCD-CLI from [argo-cd/releases](https://github.com/argoproj/argo-cd/releases).

Next install ArgoCD into your Kubernetes cluster

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
