---
description: >-
  Automatically create Harness connectors for projects and IAM roles in each GCP
  subscription
---


# Connectors + Roles For GCP CCM

The process below defines how to provision Harness connectors and GCP IAM roles using Terraform.

## Permissions <a href="#permissions" id="permissions"></a>

You will need access to provision IAM roles in GCP and create CACM connectors in Harness. When running the Terraform code, two variables need to be defined:

`HARNESS_ACCOUNT_ID` = (Your Harness Account ID)

`HARNESS_PLATFORM_API_KEY` = Created via a [service account](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control/add-and-manage-service-account) with connector and CCM admin permissions across all resources.

## Setup providers <a href="#setup-providers" id="setup-providers"></a>

We need to leverage the GCP and Harness Terraform providers. We will use these to create IAM roles and CACM connectors. We also will get all GCP subscriptions and set the Harness service account. To get all subscriptions, we are filtering on the parent folder [This](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/projects) document describes other ways to get a list of projects such as at the organization level.

You should already have a billing connector in your GCP organization for the project that has the billing export. To find this connector, go to the Harness UI -> Account Settings -> Connectors -> Click on the GCP connector that has the billing export -> Toggle the YAML view -> Copy the value for 'serviceAccountEmail'.

You can get the hierarchical structure of a project by running this gcloud CLI command:

```bash
gcloud projects get-ancestors {projectId}
```

You can get the complete project list in your organization by running:

```bash
gcloud projects list
```

```hcl
terraform {
  required_providers {
    harness = {
      source = "harness/harness"
    }
    google = {
      source = "google"
    }
  }
}

provider "google" {}

provider "harness" {}

variable "harness_gcp_sa" {
  type = string
}
```

## Get projects and create connectors <a href="#get-projects-and-create-connectors" id="get-projects-and-create-connectors"></a>

There are two options to retrieve the projects we want to create connectors for. We will use the Harness provider to create a CACM connector for each GCP project after we retrieve them. We are enabling recommendations (VISIBILITY), governance (GOVERNANCE), and autostopping (OPTIMIZATION).

### Use the Google provider to get all projects in the organization <a href="#use-the-google-provider-to-get-all-projects-in-the-organization" id="use-the-google-provider-to-get-all-projects-in-the-organization"></a>

Get all projects in a specific folder:

```hcl
data "google_projects" "my-org-projects" {
  filter = "parent.type:folder parent.id:0123456789"
}
```

Get all projects in an organization:

```hcl
data "google_projects" "my-org-projects" {
  filter = "name:*"
}
```

```hcl
resource "harness_platform_connector_gcp_cloud_cost" "this" {
  for_each = { for project in data.google_projects.my-org-projects.projects : project.project_id => project }

  identifier = replace(each.value.project_id, "-", "_")
  name       = each.value.name

  features_enabled      = ["VISIBILITY", "OPTIMIZATION", "GOVERNANCE"]
  gcp_project_id        = each.value.project_id
  service_account_email = var.harness_gcp_sa
}
```

### Use the built in locals value to define the projects statically <a href="#use-the-built-in-locals-value-to-define-the-projects-statically" id="use-the-built-in-locals-value-to-define-the-projects-statically"></a>

This is useful when you do not have a solid naming convention and you want to apply certain features to different projects. For example, you want to only apply autostopping in non-prod projects. This is also useful when you cannot authenticate to the GCP organization.

```hcl
locals {
  gcp-non-prod = ['project-1', 'project-2']
  gcp-prod = ['project-3', 'project-4']
}

resource "harness_platform_connector_gcp_cloud_cost" "this" {
  for_each = toset(concat(local.gcp-non-prod, local.gcp-prod))

  identifier = "gcp${replace(replace(trimspace(each.key), "-", "_"), " ", "_")}"
  name = "gcp${replace(replace(trimspace(each.key), "-", "_"), " ", "_")}"

  features_enabled      = ["VISIBILITY", "OPTIMIZATION", "GOVERNANCE"]
  gcp_project_id        = each.key
  service_account_email = var.harness_gcp_sa
}
```

## Create roles in each GCP project <a href="#create-roles-in-each-gcp-project" id="create-roles-in-each-gcp-project"></a>

Your organization probably already has a process to do this. When this is the case, defer to that process. Below is an alternative.

## Create role in each GCP project via Terraform <a href="#create-role-in-each-gcp-project-via-terraform" id="create-role-in-each-gcp-project-via-terraform"></a>

There are two examples. One is project-wide viewer (read-only) access and the other is project-wide editor access. Based on your needs in Harness, choose the minimum amount of permissions needed.

Note: If you give the Harness service account the appropriate permissions across your entire organization via the GCP console, you do not have to use the below Terraform to give permissions for each project. These are written using the Google provider to get all projects. If you are using the locals value to statically define the projects, the logic for the loop and project will have to be modified.

```hcl
# for view access
resource "google_project_iam_member" "viewer" {
  for_each = { for project in data.google_projects.my-org-projects.projects : project.project_id => project }

  project = each.value.project_id
  role    = "roles/viewer"
  member  = "serviceAccount:${var.harness_gcp_sa}"
}

# for editor access
resource "google_project_iam_member" "editor" {
  for_each = { for project in data.google_projects.my-org-projects.projects : project.project_id => project }

  project = each.value.project_id
  role    = "roles/editor"
  member  = "serviceAccount:${var.harness_gcp_sa}"
}
```

## Conclusion <a href="#conclusion" id="conclusion"></a>

This is a general example of providing either viewer or reditor access for each connector inside of a GCP folder. This example does not include setting up the connector for the billing export. This guide assumes there already exists a connector in a GCP project that has the billing export and an existing connector for the billing data has already registered and imported the Harness service account in the organization.

## Supplemental information <a href="#supplemental-information" id="supplemental-information"></a>

For more information, go to [Harness Terraform provider documentation](https://registry.terraform.io/providers/harness/harness/latest/docs).

For more information, go to [GCP Terraform provider documentation](https://registry.terraform.io/providers/hashicorp/google/latest/docs).

For more information, go to [GCP API documentation](https://cloud.google.com/resource-manager/docs/apis).

{% @harness-feedback/feedback %}
