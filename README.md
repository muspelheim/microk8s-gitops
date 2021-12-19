# microk8s-gitops

https://pre-commit.com/index.html

export $(grep -v '^#' .env | xargs)

kubectl create namespace flux-system --dry-run=client -o yaml | kubectl apply -f -


flux create source helm k8s-at-home --url=https://k8s-at-home.com/charts/
flux create source helm hashicorp --url=https://helm.releases.hashicorp.com


flux completion bash > ~/.fluxrc
kustomize completion bash > ~/.kustomizerc
helm completion > ~/.helmrc

if [ -f ~/.kustomizerc ]; then
. ~/.kustomizerc
fi

if [ -f ~/.fluxrc ]; then
. ~/.fluxrc
fi

if [ -f ~/.helmrc ]; then
. ~/.helmrc
fi

kubectl apply --kustomize=./cluster/base/flux-system


## :open_file_folder:&nbsp; Repository structure

The Git repository contains the following directories under `cluster` and are ordered below by how Flux will apply them.

- **base** directory is the entrypoint to Flux
- **crds** directory contains custom resource definitions (CRDs) that need to exist globally in your cluster before anything else exists
- **core** directory (depends on **crds**) are important infrastructure applications (grouped by namespace) that should never be pruned by Flux
- **apps** directory (depends on **core**) is where your common applications (grouped by namespace) could be placed, Flux will prune resources here if they are not tracked by Git anymore

```
cluster
├── apps
│   ├── default
│   ├── networking
│   └── system-upgrade
├── base
│   └── flux-system
├── core
│   ├── cert-manager
│   ├── metallb-system
│   ├── namespaces
│   └── system-upgrade
└── crds
    └── cert-manager
```