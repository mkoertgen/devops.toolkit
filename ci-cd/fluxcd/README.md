# FluxCD

- [flux-operator](https://operatorhub.io/operator/flux)

## Tutorial

```shell
# 1. Have a personal access token with repo permissions at https://github.com/settings/tokens
export GITHUB_TOKEN=<your-token>
export GITHUB_OWNER=<you or your org on github>

# 2. Have flux bootstrap itself into the repo atBootstrap flux
flux bootstrap github --owner $GITHUB_OWNER --repository hello.gitops --branch main --path ci-cd/_apps --personal true

# 3. Create a git source - have flux watch this repo for changes
flux create source git staging --url https://github.com/mkoertgen/hello.gitops --branch main --interval 30s --export > staging.yaml
flux create kustomization staging --source staging --path "./" --prune true --validation client --interval 10m --export >> staging.yaml

# 4. git add, commit & push and watch what flux thinks about our changes...
git add .
git comit -m "Add staging environment"
git push

watch flux get sources git

watch flux get kustomizations
```

## References

- [fluxcd/flux2-kustomize-helm-example](https://github.com/fluxcd/flux2-kustomize-helm-example)

See also Victor Farcic's ([vfarcic](https://twitter.com/vfarcic)) intro on

- YouTube: [Flux CD v2 with GitOps Toolkit](https://www.youtube.com/watch?v=R6OeIgb7lUI)
- Gist: [vfarcic/06-02-flux-short.sh](https://gist.github.com/vfarcic/0431989df4836eb82bdac0cc53c7f3d6)
