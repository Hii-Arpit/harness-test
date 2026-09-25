---
description: >-
  Install the Harness Delegate scoped to a dedicated namespace with a
  least-privilege Role and ClusterRole, instead of the default cluster-admin.
---


# Limited Permissions install

Use this install when `cluster-admin` is not acceptable on the target cluster. You scope the Delegate to a dedicated namespace, replace the default `ClusterRoleBinding` with a namespace-scoped `Role`, and grant chaos access to only the workloads you plan to inject chaos into through an opt-in `ClusterRole`.

## Step 1. Create a dedicated namespace <a href="#step-1-create-a-dedicated-namespace" id="step-1-create-a-dedicated-namespace"></a>

Create a dedicated namespace for the Harness Delegate. For example, `harness-delegate-ng`.

```bash
kubectl create ns harness-delegate-ng
```

## Step 2. Remove the cluster role binding from the Delegate manifest <a href="#step-2-remove-the-cluster-role-binding-from-the-delegate-manifest" id="step-2-remove-the-cluster-role-binding-from-the-delegate-manifest"></a>

Edit the Delegate Helm values (or YAML manifest) and remove the `ClusterRoleBinding` whose `roleRef.name` is `cluster-admin`. In the default Helm chart this resource is named `<delegate-name>-cluster-admin`. Removing it stops the Delegate from inheriting cluster-wide privileges.

![Cluster role binding to remove in the Delegate manifest](../../../../.gitbook/assets/cluster.png)

## Step 3. Create a new service account for the Delegate <a href="#step-3-create-a-new-service-account-for-the-delegate" id="step-3-create-a-new-service-account-for-the-delegate"></a>

Create a service account in the dedicated namespace. The Delegate pod will run as this service account.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: chaos-delegate
  namespace: harness-delegate-ng
```

## Step 4. Attach the service account to the Delegate <a href="#step-4-attach-the-service-account-to-the-delegate" id="step-4-attach-the-service-account-to-the-delegate"></a>

Reference the service account in the Delegate Helm values or manifest.

![Service account attached in the Delegate manifest](../../../../.gitbook/assets/attach.png)

## Step 5. Apply chaos RBAC <a href="#step-5-apply-chaos-rbac" id="step-5-apply-chaos-rbac"></a>

Apply the **Dedicated delegate approach (Delegate in target cluster)** manifest set from [Cluster permissions → Example RBAC manifests](../permissions.md#example-rbac-manifests). The set contains:

1. A namespace `Role` and `RoleBinding` so the Delegate can manage chaos runner pods inside `harness-delegate-ng`.
2. A `ClusterRole` (`chaos-clusterrole`) with the discovery and chaos permissions the runner needs.
3. Either a `ClusterRoleBinding` (chaos can target any namespace) or per-namespace `RoleBinding`s (chaos can target only onboarded namespaces).

The manifests assume `chaos-delegate` as the service account and `harness-delegate-ng` as the namespace. Adjust both if you used different names in Steps 1 and 3.

{% hint style="info" %}
**PICK A BINDING MODE**

* **Bind to all namespaces** with a `ClusterRoleBinding`. Easier to manage; less precise.
* **Bind to specific namespaces** with one `RoleBinding` per application namespace. Explicit per-app onboarding.
{% endhint %}

## Step 6. Create a Kubernetes connector that uses Delegate permissions <a href="#step-6-create-a-kubernetes-connector-that-uses-delegate-permissions" id="step-6-create-a-kubernetes-connector-that-uses-delegate-permissions"></a>

In Harness, create a [Kubernetes Direct Connection connector](https://developer.harness.io/harness-platform/use-harness-platform/connectors/cloud-providers/ref-cloud-providers/kubernetes-cluster-connector-settings-reference) that authenticates via the Delegate's own credentials. The Delegate's service account drives the connection.

![Kubernetes connector with Delegate credentials](../../../../.gitbook/assets/delegate-perms.png)

![Connector setup with the Delegate](../../../../.gitbook/assets/delegate-setup.png)

## Step 7. Create the Kubernetes infrastructure <a href="#step-7-create-the-kubernetes-infrastructure" id="step-7-create-the-kubernetes-infrastructure"></a>

Create the chaos infrastructure using the connector from Step 6. The form fields are identical to the [Basic install](limited-permissions.md#basic--create-a-kubernetes-infrastructure) flow.

## Step 8. Edit the infrastructure to use the dedicated namespace <a href="#step-8-edit-the-infrastructure-to-use-the-dedicated-namespace" id="step-8-edit-the-infrastructure-to-use-the-dedicated-namespace"></a>

After saving, open the infrastructure and edit it so the chaos runner is created in `harness-delegate-ng` (the namespace where the Delegate runs) using the `chaos-delegate` service account. This ensures the runner picks up the namespace-scoped `Role` from Step 5.

![Edit infrastructure to set namespace and service account](../../../../.gitbook/assets/edit-infra-sa.png)

{% @harness-feedback/feedback %}
