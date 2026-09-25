---
description: >-
  Configure AWS SAM services in Harness: SAM directory manifests, values.yaml
  Go templating, and service variables.
title: SAM services
sidebar_position: 2
nodeTitle: SAM services
---


# SAM services

### What you will learn <a href="#what-you-will-learn" id="what-you-will-learn"></a>

* How to create an AWS SAM service with a SAM directory manifest sourced from Git or S3.
* How to configure an optional values YAML manifest for Go template substitution in your SAM template.

***

A Harness service for AWS SAM represents the serverless application you want to deploy. It holds the SAM directory manifest (your `template.yaml` and supporting source code), optional values files for Go templating, and service-level variables.

Services are independent of pipelines; configure one once and reuse it across stages and pipelines.

***

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

Before you create a SAM service, make sure you have the following:

* **AWS connector:** A Harness AWS connector with permissions to call CloudFormation and S3 APIs in the target region. Go to [AWS connector settings reference](https://developer.harness.io/continuous-delivery) to set up a connector.
* **SAM application source:** A Git repository (Harness Code, GitHub, GitLab, Bitbucket, or Azure Repos) or AWS S3 bucket containing your `template.yaml` and any source artifacts.
* **Container registry connector:** A Harness connector to the container registry where your SAM plugin image is hosted.

***

### Create a SAM service <a href="#create-a-sam-service" id="create-a-sam-service"></a>

You can create a SAM service from the standalone **Services** page or inline while configuring the **Service** tab in the pipeline stage wizard. Both paths follow the same two steps.

**Step 1: Create the service**

1. Go to **Services** and select **+ New Service**, or select **+ New Service** on the **Service** tab when setting up a stage.
2. Select **AWS SAM** from the **Deployment Target** drop-down.
3. Enter a name. Harness auto-generates an ID from the name.
4. Under **Storage**, choose **Inline** (Harness stores the configuration) or a Git repository.
5. Select **Create**.

**Step 2: Configure manifests and variables**

After the service is created, go to **Configuration** and select **Edit**.

6. Under **Manifest**, select **+ Add** to add your SAM directory manifest and an optional values file. Go to [Add a SAM directory](#add-a-sam-directory) and [Add a Values YAML manifest](#add-a-values-yaml-manifest) for details.
7. Under **Variables**, select **+ Add** to define service variables. Go to [Advanced service options](#advanced-service-options) for details.
8. Select **Save**.

***

### Add a SAM directory <a href="#add-a-sam-directory" id="add-a-sam-directory"></a>

Select **+ Add** under **Manifest** and select **AWS SAM Directory**. A SAM directory manifest points to the folder in your source that contains `template.yaml`. Harness downloads this directory before running `sam build`.

#### Git providers <a href="#git-providers" id="git-providers"></a>

Use a Git provider when your SAM application lives in a Harness Code, GitHub, GitLab, Bitbucket, or Azure Repos repository.

| Field               | Description                                                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **ID**              | A unique identifier for this manifest within the service.                                                                    |
| **Source**          | Select the Git provider. Requires a Harness connector for that provider.                                                     |
| **Repository**      | The repository that contains your SAM application.                                                                           |
| **Fetch**           | Select **Branch** to track a branch head, or **Commit** to pin to a specific SHA or Git tag.                                 |
| **Branch / Commit** | The branch name or commit SHA to fetch.                                                                                      |
| **Folder Path**     | The path to the folder containing `template.yaml` from the root of the repository. For example, `apps/my-sam-app/`.         |

#### AWS S3 <a href="#aws-s3" id="aws-s3"></a>

Use S3 when your SAM application is stored as a `.zip` archive or folder in an S3 bucket.

| Field                | Description                                                                                                                  |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **ID**               | A unique identifier for this manifest within the service.                                                                    |
| **AWS Connector**    | A Harness AWS connector with `s3:GetObject` and `s3:ListBucket` permissions on the target bucket.                           |
| **Region**           | The AWS region where the bucket is located.                                                                                  |
| **Bucket Name**      | The name of the S3 bucket.                                                                                                   |
| **File/Folder Path** | The path to the folder or `.zip` file within the bucket.                                                                     |

{% hint style="info" %}
**ZIP ARCHIVE SUPPORT**

If the path points to a `.zip` file, Harness decompresses it automatically before running the build step.
{% endhint %}

***

### Add a values YAML manifest <a href="#add-a-values-yaml-manifest" id="add-a-values-yaml-manifest"></a>

SAM services support an optional Values YAML manifest for Go template substitution. A values file lets you parameterize your `template.yaml` without editing it directly. Harness resolves `{{.Values.KEY}}` references in the template before running `sam build`.

To add a values file, select **+ Add** under **Manifest** and set **Type** to **Values YAML**. The Values YAML manifest is a separate entry in the manifest list — it is not a sub-option of the SAM Directory manifest.

The form fields match the SAM Directory manifest fields. For an Amazon S3 source:

| Field                | Description                                                                                                                  |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **ID**               | A unique identifier for this manifest within the service (for example, `m2`).                                                |
| **AWS Connector**    | A Harness AWS connector with `s3:GetObject` and `s3:ListBucket` permissions on the bucket.                                   |
| **Region**           | The AWS region where the bucket is located.                                                                                  |
| **Bucket Name**      | The name of the S3 bucket containing the values file.                                                                        |
| **File/Folder Path** | The path to the `values.yaml` file within the bucket (for example, `val/values.yaml`).                                      |

Harness resolves the values file for both the SAM Build and SAM Deploy steps. To disable resolution for a specific step, set the `PLUGIN_RESOLVE_WITH_VALUES_MANIFEST` environment variable to `false` on that step.

#### Use service variables in values.yaml <a href="#use-service-variables-in-values-yaml" id="use-service-variables-in-values-yaml"></a>

You can use Harness service variable expressions as values in your `values.yaml` file. Harness resolves expressions before applying the Go template. Reference them using the format `<+serviceVariables.<name>>`.

Define service variables under **Advanced > Variables** in the service configuration. Go to [Advanced service options](#advanced-service-options) for instructions.


***

### Advanced service options <a href="#advanced-service-options" id="advanced-service-options"></a>

Expand **Advanced** on the service to access options that apply across the entire service:

* **Config Files:** Attach configuration files such as properties files, certificates, or scripts. Harness makes these available to the delegate at deploy time.
* **Variables:** Define service-level variables to parameterize your values files and pipeline expressions. Variables support fixed values, runtime inputs, and expressions. Reference them in pipelines and expressions as:
  * CEL: `${{serviceVariables.<name>}}`
  * JEXL: `<+serviceVariables.<name>>`
* **Service Hooks:** Run scripts at specific points in the service lifecycle: before or after Harness fetches manifests or applies resources.

***

### Use an existing service <a href="#use-an-existing-service" id="use-an-existing-service"></a>

When you configure a stage, select **Existing** in the **Choose service** panel. Use the **Project**, **Organization**, and **Account** tabs to select from the appropriate scope. Only AWS SAM services appear when the stage deployment type is AWS SAM.


***

### How Harness downloads manifests <a href="#how-harness-downloads-manifests" id="how-harness-downloads-manifests"></a>

When a pipeline runs, Harness executes a **Service** step before SAM Build. This step fetches every manifest defined in the service and places each one in a local directory named after its ID. For example, a manifest with ID `m1` is downloaded to `/harness/m1/`, and a Values YAML manifest with ID `m2` is downloaded to `/harness/m2/`.

The SAM Build and SAM Deploy steps then operate against these downloaded paths. You can reference a manifest's local path in pipeline expressions using `<+manifests.<id>.store.folderPath>`.

{% hint style="info" %}
**MANIFEST IDS AND LOCAL PATHS**

The ID you assign to each manifest in the service becomes the local directory name on the build runner. Use short, lowercase IDs (for example, `m1`, `values`) to keep expressions readable.
{% endhint %}

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [SAM infrastructure](sam-infrastructure.md) to configure environments and infrastructure definitions.
* Go to [AWS SAM overview](overview.md) to set up a pipeline that builds and deploys your SAM application.
* Go to [SAM Build](step-library/sam-build.md) to review the build step settings and `--use-container` requirements.

{% @harness-feedback/feedback %}
