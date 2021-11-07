# CI/CD

Comparing alternative / complementary kubernetes native CI/CD & GitOps approaches

- [tekton](https://tekton.dev/) (CI/CD)
- [argocd](https://argo-cd.readthedocs.io/en/stable/) (GitOps)
- [fluxcd](https://fluxcd.io/) (GitOps)
- [kaniko](https://github.com/GoogleContainerTools/kaniko) (k8s native docker image building)
- [skaffold](https://skaffold.dev/)

## Adding a Container Registry

For completely local development, the most straight-forward options seems to be to deploy a Docker Registry to your local Kubernetes cluster.

```shell
$ helm upgrade -i --wait docker-registry docker-registry \
  --repo https://helm.twun.io \
  -n tools tools --create-namespace
  -f ./tekton/charts/docker-registry.values.yaml
Release "docker-registry" does not exist. Installing it now.
...
NOTES:
1. Get the application URL by running these commands:
  http://registry.127-0-0-1.nip.io/

# Check endpoint
$ curl http://registry.127-0-0-1.nip.io/v2/_catalog
{"repositories":[]}
```

## Example Apps

Some example apps.

For example, build the [astro](#astro)-app like this using [Docker Compose](https://docs.docker.com/compose/) or [Skaffold](https://skaffold.dev/)

```shell
# 1. Using docker compose
docker compose build astro
docker compose push astro

# 2. Using Skaffold
cd ./_apps/astro
skaffold build
```

### Go

Using one of

- [Chi](https://github.com/go-chi/chi), [Echo](https://github.com/labstack/echo), [Twirp](https://github.com/twitchtv/twirp)
- [Gorm](https://github.com/go-gorm/gorm) (code-first) or [sql-boiler](https://github.com/volatiletech/sqlboiler) (schema-first)
- ...

### Astro

A static site build using [Astro](https://astro.build/)

### Rust

Using one of

- [actix-web](https://github.com/actix/actix-web), [rocket](https://github.com/SergioBenitez/rocket), [warp](https://github.com/seanmonstar/warp)

See

- [flosse/rust-web-framework-comparison](https://github.com/flosse/rust-web-framework-comparison)
