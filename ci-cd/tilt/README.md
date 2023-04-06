# Tilt

Example for Tilt using the [Tilt Avatars Sample Project](https://github.com/tilt-dev/tilt-avatars)

## How to run

```shell
# Start local Kind cluster
kind create cluster --name tilt-avatars

# Start Tilt
cd tilt-avatars
tilt up

# Clean up
tilt down
kind delete cluster --name tilt-avatars
```
