---
description: >-
  Automatically create Harness connectors for subscriptions and IAM roles in
  each Azure subscription
---


# Connectors + Roles For Azure CCM

The process below defines how to provision Harness connectors and Azure IAM roles using Terraform.

## Permissions <a href="#permissions" id="permissions"></a>

You will need access to provision IAM roles in Azure and create CACM connectors in Harness. When running the Terraform code, two variables need to be defined:

`HARNESS_ACCOUNT_ID` = (Your Harness Account ID)

`HARNESS_PLATFORM_API_KEY` = Created via a [service account](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control/add-and-manage-service-account) with connector and CCM admin permissions across all resources.

## Setup providers <a href="#setup-providers" id="setup-providers"></a>

We need to leverage the Azure and Harness Terraform providers. We will use these to create IAM roles and CACM connectors. We also will get all Azure subscriptions and set the Harness principal id.

```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.0.0"
    }
    harness = {
      source = "harness/harness"
    }
  }
}

provider "azurerm" {
  features {}
}

provider "harness" {}

variable "harness_principal_id" {
    type = string
    default = "0211763d-24fb-4d63-865d-92f86f77e908"
}
```

## Get subscriptions and create connectors <a href="#get-subscriptions-and-create-connectors" id="get-subscriptions-and-create-connectors"></a>

There are two options to retrieve the subscriptions we want to create connectors for. We will use the Harness provider to create a CACM connector for each Azure subscription after we retrieve them. We are enabling recommendations (VISIBILITY), governance (GOVERNANCE), and autostopping (OPTIMIZATION).

### Use the Azure provider to get all subscriptions in the tenant. <a href="#use-the-azure-provider-to-get-all-subscriptions-in-the-tenant" id="use-the-azure-provider-to-get-all-subscriptions-in-the-tenant"></a>

```hcl
data "azurerm_subscriptions" "available" {}

resource "harness_platform_connector_azure_cloud_cost" "subscription" {
  for_each = { for subscription in data.azurerm_subscriptions.available.subscriptions : subscription.subscription_id => subscription }

  identifier = "azure${replace(each.value.subscription_id, "-", "_")}"
  name       = each.value.display_name
  
  features_enabled = ["VISIBILITY", "OPTIMIZATION", "GOVERNANCE"]
  tenant_id        = each.value.tenant_id
  subscription_id  = each.value.subscription_id
}
```

### Use the built in locals value to define the subscriptions statically <a href="#use-the-built-in-locals-value-to-define-the-subscriptions-statically" id="use-the-built-in-locals-value-to-define-the-subscriptions-statically"></a>

This is useful when you do not have a solid naming convention and you want to apply certain features to different subscriptions. For example, you want to only apply autostopping in non-prod subscriptions. This is also useful when you cannot authenticate to the Azure tenant.

```hcl
locals {
  azure-non-prod = ["00000000-0000-0000-0000-000000000001", "00000000-0000-0000-0000-000000000002"]
  azure-prod = ["00000000-0000-0000-0000-000000000003", "00000000-0000-0000-0000-000000000004"]
}

resource "harness_platform_connector_azure_cloud_cost" "subscription" {
  for_each = toset(concat(local.azure-non-prod, local.azure-prod))

  identifier = "azure${replace(each.key, "-", "_")}"
  name       = "azure${replace(each.key, "-", "_")}"
  
  features_enabled = ["VISIBILITY", "OPTIMIZATION", "GOVERNANCE"]
  tenant_id        = "00000000-0000-0000-0000-000000000005""
  subscription_id = trimspace(each.key)
}
```

## Create roles in each Azure subscription <a href="#create-roles-in-each-azure-subscription" id="create-roles-in-each-azure-subscription"></a>

Your organization probably already has a process to do this. When this is the case, defer to that process. Below is an alternative.

### Create roles in each Azure subscription via Terraform <a href="#create-roles-in-each-azure-subscription-via-terraform" id="create-roles-in-each-azure-subscription-via-terraform"></a>

There are two examples. One is subscription-wide reader access and the other is subscription-wide contributor access. Based on your needs in Harness, choose the minimum amount of permissions needed.

Note: If you give the Harness principal id the appropriate permissions across your entire tenant via the Azure portal, you do not have to use the below Terraform to give permissions for each subscription. These are written using the Azure provider to get all subscriptions. If you are using the locals value to statically define the subscriptions, the logic for the loop and scope will have to be modified.

```hcl
# for view access
resource "azurerm_role_assignment" "viewer" {
  for_each = { for subscription in data.azurerm_subscriptions.available.subscriptions : subscription.subscription_id => subscription }
  
  scope                = each.value.id
  role_definition_name = "Reader"
  principal_id         = var.harness_principal_id
}
  
 # for editor access
resource "azurerm_role_assignment" "editor" {
  for_each = { for subscription in data.azurerm_subscriptions.available.subscriptions : subscription.subscription_id => subscription }
  
  scope                = each.value.id
  role_definition_name = "Contributor"
  principal_id         = var.harness_principal_id
}
```

## Conclusion <a href="#conclusion" id="conclusion"></a>

This is a general example of providing either reader or contributor access for each connector inside of an Azure tenant. This example does not include setting up the connector for the billing export. This guide assumes there already exists a connector in an Azure subscription that has the billing export and an existing connector for the billing data has already registered and imported the Harness app into the tenant.

## Supplemental information <a href="#supplemental-information" id="supplemental-information"></a>

For more information, go to [Harness Terraform provider documentation](https://registry.terraform.io/providers/harness/harness/latest/docs).

For more information, go to [Azure Terraform provider documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs).

For more information, go to [Azure REST API documentation](https://learn.microsoft.com/en-us/rest/api/azure/).

{% @harness-feedback/feedback %}
