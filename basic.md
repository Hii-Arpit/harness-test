---
description: >-
  Standard install of the Harness Delegate on the target cluster, using the
  default cluster-admin role.
---


# Basic install

Use this install when you can grant the Delegate the default `cluster-admin` role. The chaos runner inherits that role and can target any namespace in the cluster.

## Install the Harness Delegate <a href="#install-the-harness-delegate" id="install-the-harness-delegate"></a>

Follow the platform install guide at [Install Delegate](https://developer.harness.io/harness-platform/use-harness-platform/delegates/delegate/install-delegates/overview). The recommended path is the Helm chart with the **standard Delegate image** (it ships `kubectl` and `go-template`, both of which chaos needs).

{% hint style="info" %}
**USING THE MINIMAL DELEGATE IMAGE**

The minimal Delegate image does not include `kubectl` or `go-template`, so chaos and discovery do not work on it out of the box. Go to [Using the minimal Delegate image](../#using-the-minimal-delegate-image) for the two install options (`INIT_SCRIPT` or custom image).
{% endhint %}

## Create a Kubernetes infrastructure <a href="#create-a-kubernetes-infrastructure" id="create-a-kubernetes-infrastructure"></a>

1. Go to **Resilience Testing → Project Settings → Resilience Testing Infrastructures**.
2. Select the **Kubernetes (Harness Infrastructure)** tab.
3. Click **+ New Infrastructure**. The **Create a Chaos Infrastructure with a Harness Delegate** dialog opens.
4. Pick the **environment** the infrastructure belongs to. The environment picker lists project, organization, and account-scoped environments. Click **+ New Environment** if you need one. Click **Continue**.
5. In the **Create New Infrastructure** form:
   * **Name, Description, Tags.** Standard Harness metadata.
   * **Storage:** choose **Inline** (definition stored in Harness) or **Remote** (definition stored in a Git repository).
   * **Deployment Type:** Kubernetes.
   * **Infrastructure Type:** Direct Connection (Kubernetes) or **Via Cloud Provider**.
   * **Cluster Details:**
     * **Connector:** the Kubernetes connector that reaches the target cluster.
     * **Namespace:** where the chaos runner and fault pods are created.
     * **Release name (Advanced):** default `release-<+INFRA_KEY_SHORT_ID>`. Leave unchanged unless you have a naming convention.
   * **Map Dynamically Provisioned Infrastructure:** enable when the target cluster is provisioned through an upstream pipeline.
   * **Scope to Specific Services:** enable to limit chaos to a subset of services within the namespace.
   * **Allow simultaneous deployments on the same infrastructure:** leave off unless you knowingly need parallel runs.
6. Click **Save**. The new infrastructure appears in the list with status **Inactive** until you enable chaos on it.

{% hint style="info" %}
**YAML MODE**

The Visual form has a **YAML** toggle. Switching to YAML shows the `infrastructureDefinition.yaml` produced by the form, which you can edit directly or commit to Git.
{% endhint %}

{% hint style="info" %}
**TERRAFORM ALTERNATIVE**

To create a Kubernetes infrastructure through Terraform instead, use the [`harness_chaos_infrastructure_v2` resource](https://registry.terraform.io/providers/harness/harness/latest/docs/resources/chaos_infrastructure_v2) in the [Harness Terraform provider](https://developer.harness.io/harness-platform/use-harness-platform/automation/terraform-provider/harness-terraform-provider-overview).
{% endhint %}

## Onboard services on the new infrastructure <a href="#onboard-services-on-the-new-infrastructure" id="onboard-services-on-the-new-infrastructure"></a>

After you save the infrastructure, the **Onboard a new resilience testing infrastructure** wizard opens with Step 1 (Select infrastructure) already complete.

1. In Step 2, select the **Service onboarding** card. Harness discovers the services running in the infrastructure, associates probes with them, and onboards them for testing.
2. Select **Go!** to run onboarding with the default settings. To override the defaults for the [chaos runner and Discovery Agent](https://developer.harness.io/harness-platform/use-harness-platform/service-discovery/customize-agent), select **Configure Advanced Settings** first and complete the optional Step 3.

Go to [Automated service onboarding](../../../../shared-capabilities/services/service-discovery.md) for a walkthrough of the discovery, scanning, onboarding, and report stages.

The infrastructure status flips to **Active** when the Delegate registers the chaos runner and the discovery agent finishes its first sweep.

{% @harness-feedback/feedback %}
