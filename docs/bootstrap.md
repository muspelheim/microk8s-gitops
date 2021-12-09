# Bootstrapping the cluster for GitOps management of deployments

```bash
flux install \
  --version=latest \
  --components=source-controller,kustomize-controller,helm-controller,notification-controller \
  --network-policy=false \
  --export > ./flux-system/toolkit-components.yaml
```

## Apply the toolkit-components manifest

```bash
kubectl apply -f ./flux-system/gotk-components.yaml
```

## Apply the repo sync configuration

```bash
kubectl apply -f ./flux-system/gotk-sync.yaml
```