# microk8s-gitops

https://pre-commit.com/index.html

export $(grep -v '^#' .env | xargs)

flux create source helm k8s-at-home --url=https://k8s-at-home.com/charts/
flux create source helm hashicorp --url=https://helm.releases.hashicorp.com



kubectl apply --kustomize=./cluster/base/flux-system


## :open_file_folder:&nbsp; Repository structure

The Git repository contains the following directories under `cluster` and are ordered below by how Flux will apply them.

- **base** directory is the entrypoint to Flux
- **crds** directory contains custom resource definitions (CRDs) that need to exist globally in your cluster before anything else exists
- **core** directory (depends on **crds**) are important infrastructure applications (grouped by namespace) that should never be pruned by Flux
- **apps** directory (depends on **core**) is where your common applications (grouped by namespace) could be placed, Flux will prune resources here if they are not tracked by Git anymore

```
.
├── cluster
│   ├── apps
│   │   ├── default
│   │   │   └── hajimari
│   │   ├── home
│   │   │   ├── adguard-home
│   │   │   ├── esphome
│   │   │   ├── focalboard
│   │   │   ├── home-assistant
│   │   │   ├── mosquitto
│   │   │   └── zigbee2mqtt
│   │   └── networking
│   │       ├── authelia
│   │       ├── linkerd
│   │       └── traefik
│   ├── base
│   │   └── flux-system
│   ├── core
│   │   ├── cert-manager
│   │   ├── kubed
│   │   ├── metallb-system
│   │   └── namespaces
│   └── crds
│       ├── cert-manager
│       └── traefik
└── standalone
    └── adguard
```
