# microk8s-gitops

https://pre-commit.com/index.html

export $(grep -v '^#' .env | xargs)


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