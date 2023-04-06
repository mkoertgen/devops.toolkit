# Security

## Sealed Secrets

One approach is [bitnami/sealed-secrets](https://github.com/bitnami-labs/sealed-secrets).
Here is how to install the [sealed-secrets-operator](https://operatorhub.io/operator/sealed-secrets-operator-helm)

```shell
kubectl create -f https://operatorhub.io/install/sealed-secrets-operator-helm.yaml

kubectl get csv -n my-sealed-secrets-operator-helm

```

## References

See

- [Automation Assistants: GitOps Tools in comparison](https://cloudogu.com/en/blog/gitops-tools) (BlogPost 03/17/2021)
- [Chaosblade-operator]((./chaosblade/README.md): A Chaos Engineering Tool for Cloud-native
- [litmuschaos/litmus](https://github.com/litmuschaos/litmus) (Cloud-Native Chaos Engineering)
- Mozilla [SOPS](https://github.com/mozilla/sops): Simple and flexible tool for managing secrets
- [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets): A Kubernetes controller and tool for one-way encrypted Secrets
- [Kamus](https://github.com/Soluto/kamus): An open source, git-ops, zero-trust secret encryption and decryption solution for Kubernetes applications
