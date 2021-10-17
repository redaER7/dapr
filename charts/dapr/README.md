# Introduction

This chart deploys the Dapr control plane system services on a Kubernetes cluster using the Helm package manager.

## Chart Details

This chart installs Dapr via "child-charts" from :
https://github.com/dapr/dapr/tree/master/charts/dapr


## Why edit Chart ?

We want to have two dapr configurations on two namespaces `dev` and `uat` in the same cluster.

Dapr chart is succefully installed on `dev` namespace, when installing a second dapr chart on `uat` namespace, we get ClusterRole ClusterRoleBinding webhook errors because existing chart applies cluster-wide ressource

==> Customizing chart so that dapr helm chart installation can be namespaced

### Edit sub-chart dapr_rbac

RBAC ressources names from `ClusterRoleBinding.yaml` are updated with different names so they do not conflict whith pre-existing RBAC ressources related to first installation on `dev`.

### Edit dapr_sidecar_injector

MutatingWebhookConfiguration ressource from `dapr_sidecar_injector_webhook_config.yaml`  name is updated with different so it doesn't conflict with pre-existing MutatingWebhookConfiguration related to first installation on `dev`.

### Installation

```bash
helm upgrade -i --namespace uat \
        dapr-uat dapr-custom/ \
        -f dapr-values.yaml
```
