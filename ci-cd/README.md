# CI/CD

Comparing alternative / complementary kubernetes native CI/CD & GitOps approaches

- [tekton](https://tekton.dev/) (CI/CD)
- [argocd](https://argo-cd.readthedocs.io/en/stable/) (GitOps)
- [fluxcd](https://fluxcd.io/) (GitOps)
- [kaniko](https://github.com/GoogleContainerTools/kaniko) (k8s native docker image building)
- [skaffold](https://skaffold.dev/)

## Example Apps

Some example apps to build

### Go

Using one of

- [Chi](https://github.com/go-chi/chi), [Echo](https://github.com/labstack/echo), [Twirp](https://github.com/twitchtv/twirp)
- [Gorm](https://github.com/go-gorm/gorm) (code-first) or [sql-boiler](https://github.com/volatiletech/sqlboiler) (schema-first)
- ...

### Rust

Using one of

- [actix-web](https://github.com/actix/actix-web), [rocket](https://github.com/SergioBenitez/rocket), [warp](https://github.com/seanmonstar/warp)

See

- [flosse/rust-web-framework-comparison](https://github.com/flosse/rust-web-framework-comparison)
