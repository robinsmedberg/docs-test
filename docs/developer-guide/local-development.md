# Local development workflow

## Using Tilt

- Install local kubernetes cluster

    [Rancher Desktop](https://docs.rancherdesktop.io/getting-started/installation/)

    or

    [Docker Desktop](https://www.docker.com/products/docker-desktop/) and make sure to enable Kubernetes

- [Install tilt](https://docs.tilt.dev/install.html)

- [Add Tilfile](https://docs.tilt.dev/tiltfile_authoring.html) (only if not exists)

- Ensure Kubernets context is your local cluster

```bash
# Rancher Desktop
kubectl config use-context rancher-desktop

# or Docker Desktop
kubectl config use-context docker-desktop
```

- Start local cluster

- Run tilt

```bash
Tilt up
```

### Running multiple applications / services

```bash
Tilt up --port {port}
```
