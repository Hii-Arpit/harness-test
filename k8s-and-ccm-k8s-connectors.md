---
description: >-
  Automatically create Harness connectors that are required to get K8s costs
  into CACM
---


# Connectors For K8s and CACM K8s

The process below defines how to provision Harness connectors to get K8s costs into CACM.

## Permissions <a href="#permissions" id="permissions"></a>

You will need access to create CACM connectors in Harness. When running the Terraform code, two variables need to be defined:

`HARNESS_ACCOUNT_ID` = (Your Harness Account ID)

`HARNESS_PLATFORM_API_KEY` = Created via a [service account](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control/add-and-manage-service-account) with connector and CACM admin permissions across all resources.

## Setup providers <a href="#setup-providers" id="setup-providers"></a>

We need to leverage the Harness Terraform provider. We will use this provider to create two connectors for each cluster. We will also define all of our cluster names within Terraform `locals` block. The cluster names will be used to create the connectors.

```hcl
terraform {
  required_providers {
    harness = {
      source = "harness/harness"
    }
  }
}

provider "harness" {}

locals {
    cluster_names = toset(["cluster-a", "cluster-b", "cluster-c"])
}
```

## Create a K8s connector for each cluster <a href="#create-a-k8s-connector-for-each-cluster" id="create-a-k8s-connector-for-each-cluster"></a>

This is the first connector we need to provision. It addresses the question: "How do I connect to the delegate in the cluster"?

```hcl
resource "harness_platform_connector_kubernetes" "cluster-connector" {
  for_each = local.cluster_names
  identifier  = replace(each.value, "-", "_")
  name        = replace(each.value, "_", "-")

  inherit_from_delegate {
    delegate_selectors = [each.value]
  }
}
```

## Create a CACM K8s connector for each cluster <a href="#create-a-cacm-k8s-connector-for-each-cluster" id="create-a-cacm-k8s-connector-for-each-cluster"></a>

This connector can only be provisioned after the K8s connector. It is in charge of letting the process know we need to start gathering metrics for CACM.

```hcl
resource "harness_platform_connector_kubernetes_cloud_cost" "ccm-cluster-connector" {
  for_each = local.cluster_names
  identifier  = "${replace(each.value, "-", "_")}_ccm"
  name        = "${replace(each.value, "_", "-")}-ccm"

  features_enabled = ["VISIBILITY", "OPTIMIZATION"]
  connector_ref    = harness_platform_connector_kubernetes.cluster-connector[each.key].id
}
```

## Conclusion <a href="#conclusion" id="conclusion"></a>

This is a general example of taking a list of clusters and creating the two necessary connectors to get K8s costs into CACM. Before you create these connectors, you need to be sure you have a delegate running in the cluster. If the EKS clusters are running in AWS, the metrics server also needs to be enabled in each cluster.

{% @harness-feedback/feedback %}
