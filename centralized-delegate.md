---
description: >-
  Run one Harness Delegate on a central infrastructure that orchestrates chaos
  experiments across multiple target clusters through Kubernetes connectors.
tags:
  - chaos-engineering
  - infrastructure
  - kubernetes
  - delegate
---


# Centralized delegate approach

In the centralized delegate approach, **one Harness Delegate** runs on a central infrastructure and orchestrates chaos experiments against **multiple target clusters** through Kubernetes connectors. The Delegate itself does not need to live in the target cluster; instead, the Kubernetes connector's service account holds the chaos permissions.

This is the right pattern when a platform team owns the Delegate and many product teams contribute clusters to test against.

![Centralized Delegate communicating with target clusters](../../../.gitbook/assets/centralized-delegate.png)

***

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **The Harness Delegate is already installed** on a central infrastructure (a hub cluster, a management cluster, or any infrastructure with reachability to the target). Go to [Install Delegate](https://developer.harness.io/harness-platform/use-harness-platform/delegates/delegate/install-delegates/overview) if it is not.
* **Standard Delegate image.** If you are pinned to the minimal Delegate image, you need to install `kubectl` and `go-template` first. Go to [Using the minimal Delegate image](./#using-the-minimal-delegate-image) for the two install options.
* **Network connectivity** between the central infrastructure and every target cluster the Delegate will inject chaos into.
* **`kubectl` access** to the target cluster.
* **A Harness environment** to attach the infrastructure to. Go to [Create an environment](https://app.gitbook.com/s/lSkpbpeYJ3rfGUkIrcyQ/chaos-experiments/create-experiments#create-environment) if you do not have one.

***

## Step 1. Create the service account and chaos RBAC on the target cluster <a href="#step-1-create-the-service-account-and-chaos-rbac-on-the-target-cluster" id="step-1-create-the-service-account-and-chaos-rbac-on-the-target-cluster"></a>

On the target cluster, apply the **Centralized delegate approach (Delegate outside target cluster)** manifest set from [Cluster permissions → Example RBAC manifests](permissions.md#example-rbac-manifests). The set contains:

1. A `ServiceAccount` (`chaos-sa`) and a long-lived token `Secret` in a dedicated namespace (`harness-delegate-chaos`).
2. A namespace `Role` and `RoleBinding` so the Delegate can manage chaos runner pods inside that namespace.
3. A `ClusterRole` (`chaos-clusterrole`) with the discovery and chaos permissions the runner needs.
4. Either a `ClusterRoleBinding` (chaos can target any namespace) or per-namespace `RoleBinding`s (chaos can target only onboarded namespaces).

{% hint style="info" %}
**KEEP NAMESPACES CONSISTENT**

Harness recommends keeping the Delegate namespace, the chaos infrastructure namespace, and the service account namespace identical (`harness-delegate-chaos` in the example). This avoids cross-namespace RBAC surprises.
{% endhint %}

{% hint style="info" %}
**PICK A BINDING MODE**

* **Bind to all namespaces** with a `ClusterRoleBinding`. Easier to manage; less precise.
* **Bind to specific namespaces** with one `RoleBinding` per application namespace. Explicit per-app onboarding.
{% endhint %}

### Generate the service account token <a href="#generate-the-service-account-token" id="generate-the-service-account-token"></a>

Fetch the bound token; you will paste it into the Kubernetes connector in the next step.

```bash
kubectl -n harness-delegate-chaos get secret chaos-sa-secret \
  -o jsonpath='{.data.token}' | base64 --decode
```

***

## Step 2. Create the Kubernetes connector <a href="#step-2-create-the-kubernetes-connector" id="step-2-create-the-kubernetes-connector"></a>

In Harness, create a [Kubernetes Direct Connection connector](https://developer.harness.io/harness-platform/use-harness-platform/connectors/cloud-providers/ref-cloud-providers/kubernetes-cluster-connector-settings-reference) that authenticates with the service account token from Step 1.

* **Master URL:** run `kubectl cluster-info` on the target cluster and copy the control plane URL.
* **Service account token:** the base64-decoded value from Step 1.

![Kubernetes connector using service-account-token authentication](../../../.gitbook/assets/diff-cluster.png)

***

## Step 3. Create the Harness infrastructure definition <a href="#step-3-create-the-harness-infrastructure-definition" id="step-3-create-the-harness-infrastructure-definition"></a>

In Harness, create the chaos infrastructure using the connector from Step 2. Use the **Kubernetes (Harness Infrastructure)** tab under **Resilience Testing → Project Settings → Resilience Testing Infrastructures**. The form fields are identical to the [dedicated delegate Basic install](dedicated-delegate/#basic--create-a-kubernetes-infrastructure).

***

## Step 4. Edit the infrastructure to use the chaos namespace and service account <a href="#step-4-edit-the-infrastructure-to-use-the-chaos-namespace-and-service-account" id="step-4-edit-the-infrastructure-to-use-the-chaos-namespace-and-service-account"></a>

After enabling chaos on the infrastructure, open it and edit it so the chaos runner is launched in `harness-delegate-chaos` (or the namespace you used) with `chaos-sa` as the service account. This ensures experiments run with the bindings from Step 1.

![Service account configured on the infrastructure](../../../.gitbook/assets/sa-3.png)

***

## Next steps <a href="#next-steps" id="next-steps"></a>

* [Cluster permissions](permissions.md): the full API permission reference for the chaos service account, with copy-paste RBAC manifests.
* [Dedicated delegate approach](dedicated-delegate/): if one Delegate per cluster is acceptable.
* [Network configuration](network-config.md): mTLS and proxy settings for the Delegate and Discovery Agent.

{% @harness-feedback/feedback %}
