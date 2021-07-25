# Authentication

Secured Access via [OAuth2 Proxy](https://github.com/oauth2-proxy/oauth2-proxy) and [Dex](https://github.com/dexidp/dex)

See also [Secure Tekton dashboard behind Oauth2-Proxy](https://github.com/tektoncd/dashboard/blob/main/docs/walkthrough/walkthrough-oauth2-proxy.md)

For example purposes register an OAuth Application (e.g. as [GitHub Developer App](https://github.com/settings/developers)).

Next, install `oauth2-proxy`

```shell
helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
helm repo update

helm upgrade --install --wait --create-namespace --namespace tools oauth2-proxy oauth2-proxy/oauth2-proxy -f oauth2-proxy.values.yaml
```

## Multiple Connectors using Dex

As of now oauth2-proxy does only support a single a single connector (see [oauth2-proxy/issues/926](https://github.com/oauth2-proxy/oauth2-proxy/issues/926)).
To support multiple connectors as a true Single-Sign-On solution with your Kubernetes Cluster you can add Dex

```shell
# For federated Connectors use dex: https://artifacthub.io/packages/helm/dex/dex
helm repo add dex https://charts.dexidp.io
helm repo update

helm upgrade --install --wait --namespace tools dex dex/dex -f dex.values.yaml

# Adjust oauth2-proxy values to use Dex as OIDC connector and upgrade
helm upgrade --install --wait tools oauth2-proxy oauth2-proxy/oauth2-proxy -f oauth2-proxy.values.yaml
```
