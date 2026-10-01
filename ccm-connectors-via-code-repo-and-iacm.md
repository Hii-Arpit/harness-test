---
description: >-
  Automatically Create CACM Cloud Connectors via Harness Modules Code Repo and
  IaCM
---


# Harness Modules Code Repo + IaCM for Automatic Creation of CACM Cloud Connectors

The process below defines a system where we can use the Harness modules Code Repository and Infrastructure as Code together to accomplish creating CACM cloud connectors at scale. For this exercise, we will focus on AWS CACM Cloud Connectors, but other cloud providers could follow this same process.

To accomplish this, we will store our Terraform code in Code Repository. We will then use this repo in the IaCM module to apply the connectors.

Users of this guide should have an understanding of the Harness modules Code Repository, IaCM, and CACM.

## Setup <a href="#setup" id="setup"></a>

In the pipeline portion of this guide we will need a Kubernetes cluster a delegate running in the cluster that has permissions to deploy a pod. Do not proceed until there is confirmation this is configured.

### Create a project <a href="#create-a-project" id="create-a-project"></a>

The project will use the Code Repository and IaCM modules.

![](../../.gitbook/assets/new-project.png)

### Create a new code repository and add IaC code for connectors <a href="#create-a-new-code-repository-and-add-iac-code-for-connectors" id="create-a-new-code-repository-and-add-iac-code-for-connectors"></a>

This will be used to store and maintain our IaC.

1. Go into your new project and create a new Code repository. This will hold the code for our CACM connectors.

![](../../.gitbook/assets/new-code-repo.png)

2. Create a main.tf file that contains Terraform code. We will use two parts of this: [Part 1](../best-practices/aws/aws-connectors-and-roles.md#setup-providers) is for the providers portion and [Part 2](../best-practices/aws/aws-connectors-and-roles.md#use-the-built-in-locals-value-to-define-the-accounts-statically) for defining accounts statically.

Note: For the role\_arn, you will have to replace the role name `HarnessCERole` with whatever role name you used when you provisioned it into each AWS account for Harness to use. It has to have the appropriate permissions done correctly for each CCM feature you want to use. Setting up this roles is outside the scope of this exercise, but navigate to [Creating roles in each AWS account](../best-practices/aws/aws-connectors-and-roles.md#create-roles-in-each-aws-account) for setup options.

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    harness = {
      source = "harness/harness"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

provider "harness" {}

data "harness_platform_current_account" "current" {}

locals {
  aws-non-prod = ["000000000005", "000000000006"]
  aws-prod = ["000000000007", "000000000008"]
}

resource "harness_platform_connector_awscc" "data" {
  for_each = toset(concat(local.aws-non-prod, local.aws-prod))

  identifier = "aws${each.key}"
  name       = "aws${each.key}"

  account_id = trimspace(each.key)

  features_enabled = [
    "OPTIMIZATION",
    "VISIBILITY",
    "GOVERNANCE",
  ]
  cross_account_access {
    role_arn    = "arn:aws:iam::${each.key}:role/HarnessCERole"
    external_id = "harness:891928451355:${data.harness_platform_current_account.current.id}"
  }
}
```

### Create a new IaCM workspace <a href="#create-a-new-iacm-workspace" id="create-a-new-iacm-workspace"></a>

We will use this to store our IaC configuration, variables, states, and other resources necessary to manage our AWS CACM cloud connectors

1. Navigate to the IaCM module and create a new workspace.
   * Provisioner:
     * Connector (a few options):
       * If you have an AWS connector for your master billing account already (not a CACM AWS connector), choose this for your connector.
       * If you need to create a new connector, the suggestion is to [use OIDC](https://developer.harness.io/harness-platform/use-harness-platform/connectors/cloud-providers/ref-cloud-providers/aws-connector-settings-reference#credentials). You will have to provision a role in your master billing AWS account that trusts Harness. In setup, you can skip setting up the backoff strategy and select connect through Harness platform for the connectivity mode. You have to select a connector to complete setup. Even though we are not going to use this connector in our example (because we are getting the account ids statically in the Terraform code), we still have to specify the connector.
     * Workspace Type:
       * Choose the latest version of OpenTofu as our support for Terraform ends with 1.5.7 [due to licensing changes](https://developer.harness.io/infrastructure-as-code-management/troubleshooting-and-resources/whats-supported#supported-iac-frameworks).
   * Repository:
     * Choose Harness Code Repository and select the repository we created in the first step. Select main as the branch and the folder path should be blank as we created the main.tf in the root directory.

![](../../.gitbook/assets/ccm-iacm-new-workspace.png)

#### Define variables <a href="#define-variables" id="define-variables"></a>

The AWS authentication is handled via the OIDC connector defined above, but Harness authentication still needs to be configured. To define the Harness authentication, we need to define two environment variables: Harness account id and Harness platform API key.

For the Harness platform API Key, you will need to:

1. Create a service account
2. Give the service account, _Account Admin for all account level resources_ role. This is overpermissive. If you want, you can also create a custom role that only has connector admin.
3. Create an API key, then a token. Copy the token value
4. Create a new secret with the token

![](../../.gitbook/assets/service-account-iacm-connectors.png)

![](../../.gitbook/assets/iacm-ccm-connector-secret.png)

````
  ```text
  HARNESS_ACCOUNT_ID (string). Built in Harness variable = <+account.identifier>
  HARNESS_PLATFORM_API_KEY (secret) = New secret created from the steps above
  ```
````

![](../../.gitbook/assets/iacm-ccm-variables.png)

### Create a Terraform pipeline <a href="#create-a-terraform-pipeline" id="create-a-terraform-pipeline"></a>

Create a new pipeline. The pipeline will be used to run our init, plan, and apply Terraform stages.

1. Add a new stage. Select `Infrastructure` as the stage type and name the stage `ccm_connectors`
   * Select the infrastructure as Kubernetes, select the Kubernetes cluster you identified earlier on at the beginning of the setup portion of this guide, and choose your namespace

![](../../.gitbook/assets/iacm-connector-stage-infra.png)

2. Select the workspace we created in the step above
3. For execution, choose the 'Blank Canvas' operation
4. Add a step, select 'IACM OpenTofu Plugin'. Set the command to `init` and leave everything else the same
5. Add another step, select 'IACM OpenTofu Plugin'. Set the command to `plan` and leave everything else the same
6. Add another step, select 'IACM Approval'. Leave everything else the same
7. Add a final step, select 'IACM OpenTofu Plugin'. Set the command to `apply` and leave everything else the same
8. Save the stage

Things to consider:

1. By running this pipeline in your cluster, you are going to be pulling images into your cluster. If your company does not allow this, you will either have to:
   * Get a security exception to be able to pull from Docker Hub or
   * Mirror the Harness image into your local repository, edit the step yamls of each step to update the step specs. You will have to define the image and connector. If you do not have it already, you will have to create a Docker connector for your company repo and specify

![](../../.gitbook/assets/iacm-local-images.png)

2. You will need firewall exceptions for the steps as well. Each step must download OpenTofu at runtime. This was a conscious decision because you might have hundreds of workspaces using various OpenTofu versions, and managing all those versions would be a significant task.

## Run the pipeline <a href="#run-the-pipeline" id="run-the-pipeline"></a>

In the previous steps, we spent time going over setting up the OIDC connector to be able to read from the master billing account. This is necessary when you [want to provision a connector for each account in the organization dynamically](../best-practices/aws/aws-connectors-and-roles.md#use-the-aws-provider-to-get-all-accounts-in-the-organization). In our example we do not actually need this because if you remember our IaC code, we are defining the account ids in code statically.

1. Run the pipeline. The code will run up until the approval step

![](../../.gitbook/assets/iacm-pipeline-before-approval.png)

2. Review and approve the pipeline. In this example, I have been assigned Project Admin for all resources so I can approve the pipeline. If you want to add RBAC for who can approve your pipeline, either give them Project Admin or use the fine-grain `Approve` permission in the Infrastructure as Code section and create a custom role.

![](../../.gitbook/assets/iacm-role.png)

3. After the pipeline is complete, navigate to connectors in account setting and verify the connectors created. In the screenshot below, the status is failed only because the IAM role I am expecting is not in the accounts yet.

![](../../.gitbook/assets/connectors-in-account-settings.png)

## Schedule pipeline runs <a href="#schedule-pipeline-runs" id="schedule-pipeline-runs"></a>

You can add a Cron trigger to run the pipeline on a frequency. This is useful for when new AWS accounts get added, we can automatically run the pipeline and pick create new connectors for them.

1. Select your pipeline, select 'Triggers' on the top right of the screen, and create a new trigger.
2. Scroll to the bottom of the trigger options and select 'Cron'
3. Run it daily (or whatever you prefer)

![](../../.gitbook/assets/iacm-pipeline-cron.png)

{% @harness-feedback/feedback %}
