# hello.gitops

Comparing modern k8s / gitops approaches

- [skaffold](https://skaffold.dev/)
- [kaniko](https://github.com/GoogleContainerTools/kaniko)
- [tekton](https://tekton.dev/)

## Local Kubernetes Options

Some contemporary options to [run Kubernetes on your own machine](https://itnext.io/run-kubernetes-on-your-machine-7ee463af21a2), such as

- [Docker Desktop - Enable Kubernetes](https://docs.docker.com/desktop/kubernetes/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [microk8s](https://microk8s.io/)
- [minishift](https://www.okd.io/minishift/)
- [k3s](https://rancher.com/docs/k3s/latest/en/installation/), [k3d](https://github.com/rancher/k3d), [k3sup](https://github.com/alexellis/k3sup)

## Example Apps

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
