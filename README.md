# cna-helm-operator

This repository contains an example of a helm operator for ArgoCD. The helm chart in this operator is a forked version of [this](https://github.com/argoproj/argo-helm/commit/07b173f9672ce176218843a7c8c2a7dc8e439b43#diff-84be44642347092358794de8eb3cc56c) argo-helm commit.

## Configuration and Build Process
### Configuration
The helm chart's `values.yaml` is represented in `deploy/crds/cna_v1alpha1_argocd_cr.yaml`. Review the file and update the relevant fields.

### Deploy the CRD
```
$ kubectl create -f deploy/crds/cna_v1alpha1_argocd_crd.yaml
```
### Build and run the Operator
Build the helm-operator image and push it to a registry:

```
$ operator-sdk build quay.io/example/cna-helm-operator:v0.0.1
$ docker push quay.io/example/cna-helm-operator:v0.0.1
```

Kubernetes deployment manifests are generated in the `deploy/operator.yaml` file. The deployment image in this file needs to be modified from the placeholder REPLACE_IMAGE to the previous built image. To do this, run:
```
$ sed -i 's|REPLACE_IMAGE|quay.io/example/cna-helm-operator:v0.0.1|g' deploy/operator.yaml
```
Deploy the Operator:
```
$ kubectl create -f deploy/service_account.yaml
$ kubectl create -f deploy/role.yaml
$ kubectl create -f deploy/role_binding.yaml
$ kubectl create -f deploy/operator.yaml
```
