# Bootstrapping the cluster for GitOps management of deployments

## Requirements
```bash
apache2-utils
git
```

## Enable Microk8s addons
```bash
microk8s enable dns helm helm3 host-access storage
microk8s stop && microk8s start
```

## Create namespace
```bash
kubectl create namespace flux-system --dry-run=client -o yaml | kubectl apply -f -
```

## Create SOPS Secrets
```bash
export KEY_FP=1EB49F4A2588BBFBBCCA28B87DAE506C2FB93A3D
gpg --export-secret-keys --armor "${KEY_FP}" | kubectl create secret generic sops-gpg --namespace=flux-system --from-file=sops.asc=/dev/stdin
```

## Generate Flux2 Config
Due to race conditions with the Flux CRDs you will have to run the below command twice. There should be no errors on this second run.

Generate file only if not exist or need to update config:
```bash
flux install \
  --version=latest \
  --components=source-controller,kustomize-controller,helm-controller,notification-controller \
  --network-policy=false \
  --export > ./cluster/base/flux-system/gotk-components.yaml
```

## Apply the gotk-components manifest
```bash
kubectl apply -f ./cluster/base/flux-system/gotk-components.yaml
```

## Apply the repo sync configuration
```bash
kubectl apply -f ./cluster/base/flux-system/gotk-sync.yaml
```

## Apply the CRDS & Core components
```bash
kubectl apply -k ./cluster/crds/
```
