# microk8s-gitops


export $(grep -v '^#' .env | xargs)

curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash

flux create source helm k8s-at-home --url=https://k8s-at-home.com/charts/
flux create source helm hashicorp --url=https://helm.releases.hashicorp.com


flux completion bash > ~/.fluxrc
kustomize completion bash > ~/.kustomizerc



flux bootstrap github \
--owner=$GITHUB_USER \
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