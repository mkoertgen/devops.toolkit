# Tilt

Example for Tilt using the [Tilt Avatars Sample Project](https://github.com/tilt-dev/tilt-avatars)

## How to run

```shell
# Start local Kind cluster, check for local registry
# - https://github.com/tilt-dev/kind-local
# - up-to-date: https://kind.sigs.k8s.io/docs/user/local-registry/
export KIND_CLUSTER_NAME=tilt-avatars
./kind-with-registry.sh

# Prepare & start Tilt
git submodule update --init --recursive
cd tilt-avatars
tilt up

# Clean up
tilt down
kind delete cluster -n $KIND_CLUSTER_NAME
docker stop kind-registry && docker rm kind-registry
```
