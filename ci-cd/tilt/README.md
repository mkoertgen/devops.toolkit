# Tilt

Example for Tilt using the [Tilt Avatars Sample Project](https://github.com/tilt-dev/tilt-avatars)

## How to run

```shell
# Start local Kind cluster, check for local registry - https://github.com/tilt-dev/kind-local
export KIND_CLUSTER_NAME=tilt-avatars
kind create cluster --name tilt-avatars

# Prepare & start Tilt
git submodule update --init --recursive
cd tilt-avatars
tilt up

# Clean up
tilt down
kind delete cluster --name tilt-avatars
```
