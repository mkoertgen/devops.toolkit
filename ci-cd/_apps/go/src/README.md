# Sample Controller (Go)

## Prerequisites

Install [kubebuilder](https://book.kubebuilder.io/quick-start.html#installation) and skaffold a new project

```shell
$ kubebuilder init --domain my.domain --repo my.domain/guestbook
Writing kustomize manifests for you to edit...
Writing scaffold for you to edit...
...
go: downloading golang.org/x/mod v0.3.0
Next: define a resource with:
$ kubebuilder create api
```

## Build

```shell
$ skaffold build
Generating tags...
 - registry.gitlab.com/mkoertgen/devops-toolkit/apps/go-kubebuilder-guestbook -> registry.gitlab.com/mkoertgen/devops-toolkit/apps/go-kubebuilder-guestbook:ce8da13-dirty
Checking cache...
 - registry.gitlab.com/mkoertgen/devops-toolkit/apps/go-kubebuilder-guestbook: Found. Tagging

$ docker push registry.127-0-0-1.nip.io/myapps/go-kubebuilder-guestbook:3eefb98-dirty
The push refers to repository [registry.127-0-0-1.nip.io/myapps/go-kubebuilder-guestbook]
388380bc36ee: Layer already exists
16679402dc20: Layer already exists
3eefb98-dirty: digest: sha256:38415dcf258cc4f101e7545dd4adb8f36f4de19413a14663cd3d458741f6fd92 size: 739
```

## References

See

- [Quick Start - The KubeBuilder Book](https://book.kubebuilder.io/quick-start.html#installation)
- [kubernetes/sample-controller](https://github.com/kubernetes/sample-controller)
- [How to write Kubernetes custom controllers in Go](https://medium.com/speechmatics/how-to-write-kubernetes-custom-controllers-in-go-8014c4a04235)
- [Creating a Kubernetes operator on Windows and WSL](https://blog.baeke.info/2019/12/18/creating-a-kubernetes-operator-on-windows-and-wsl/)
