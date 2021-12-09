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

export GITHUB_TOKEN=GHTOKEN

flux create source git flux-system \
--url=ssh://git@github.com/muspelheim/microk8s-gitops \
--branch=main

flux create kustomization my-secrets \
--source=my-secrets \
--path=./ \
--prune=true \
--interval=10m \
--decryption-provider=sops \
--decryption-secret=sops-gpg


flux bootstrap github \
--owner=muspelheim \
--repository=microk8s-gitops \
--branch=main \
--path=./ \
--personal


flux create source git podinfo \
--url=https://github.com/stefanprodan/podinfo \
--branch=master \
--interval=30s \
--export > ./cluster/podinfo-source.yaml

flux create kustomization podinfo \
--source=podinfo \
--path="./kustomize" \
--prune=true \
--validation=client \
--interval=5m \
--export > ./cluster/podinfo-kustomization.yaml


helm template \
cert-manager jetstack/cert-manager \
--namespace cert-manager \
--create-namespace \
--version v1.6.1 > cert-manager.custom.yaml



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