# CI/CD

Comparing alternative / complementary kubernetes native CI/CD & GitOps approaches

- [tekton](https://tekton.dev/) (CI/CD)
- [argocd](https://argo-cd.readthedocs.io/en/stable/) (GitOps)
- [fluxcd](https://fluxcd.io/) (GitOps)
- [kaniko](https://github.com/GoogleContainerTools/kaniko) (k8s native docker image building)
- [skaffold](https://skaffold.dev/)
- [tilt.dev](https://tilt.dev/)

## Adding a Container Registry

When building images with tekton & kaniko we will need a (private) registry, e.g.

- [GitLab Container Registry](https://docs.gitlab.com/ee/user/packages/container_registry/)
- [GitHub](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

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
Generating tags...
 - registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro -> registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro:ce8da13-dirty
Checking cache...
 - registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro: Not found. Building
Starting build...
Found [docker-desktop] context, using local docker daemon.
Building [registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro]...
...
Successfully built 76e87e286b4f
Successfully tagged registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro:ce8da13-dirty
The push refers to repository [registry.gitlab.com/mkoertgen/devops-toolkit/apps/astro]
...
e62ca561a0e1: Pushed
ce8da13-dirty: digest: sha256:e0736bfb33957e2b378a951e7d76e4d919439e4048b509ebe0f8676b6fe51216 size: 1776
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
