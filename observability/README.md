# Observability

TODO...

## Fluent-Bit

There are various logging operators incoporating FluentD (see [References](#references)), yet for now we stick to the standard [fluent/helm-charts](https://github.com/fluent/helm-charts).

```shell
# Install helm chart
helm repo add fluent https://fluent.github.io/helm-charts
helm repo update

helm upgrade -i --wait --create-namespace -n monitoring fluent-bit fluent/fluent-bit -f charts/fluent-bit.values.yaml

# Configure fluent-bit (configmap, usually do this before the helm chart)
kubectl -n monitoring apply -f manifests/fluent-bit-cm.yaml
```

## Elastic Cloud

[Install ECK using the Helm chart](https://www.elastic.co/guide/en/cloud-on-k8s/1.6/k8s-install-helm.html)

```shell
helm repo add elastic https://helm.elastic.co
helm repo update

helm upgrade -i --wait --create-namespace -n operators eck elastic/eck-operator -f charts eck.values.yaml

# Add a minimal elasticsearch / kibana stack
kubectl apply -f manifests/eck-quickstart.yaml -n monitoring

# Get the (generated) password of the elastic-user
kubectl -n monitoring get secret quickstart-es-elastic-user -o=jsonpath="{.data.elastic}" | base64 --decode
```

Hit Kibana at [http://logs.127-0-0-1.nip.io](http://logs.127-0-0-1.nip.io)

## References

- [Banzai Logging Operator](https://github.com/banzaicloud/logging-operator) (commercial)
- [kubesphere/fluentbit-operator](https://github.com/kubesphere/fluentbit-operator)
- [Elastic Cloud on Kubernetes](https://www.elastic.co/guide/en/cloud-on-k8s/1.6/index.html)
