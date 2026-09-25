---
title: Trigger pipelines on a new artifact
sidebar_position: 3
keywords:
  - artifact trigger
  - on new artifact
  - artifact polling
  - trigger conditions
  - metadata conditions
  - multi-region artifact
  - lastPublished tag
helpdocs_topic_id: c1eskrgngf
helpdocs_category_id: oya6qhmmaw
helpdocs_is_private: false
helpdocs_is_published: true
canonical_url: >-
  https://www.harness.io/blog/automate-ci-cd-effortlessly-with-harness-code-repository-trigger
nodeTitle: Trigger pipelines on a new artifact
inputFilePath: docs/platform/triggers/trigger-on-a-new-artifact.md
originalUrl: https://developer.harness.io/docs/platform/triggers/trigger-on-a-new-artifact/
description: >-
  Trigger Harness Pipeline deployments in response to a new artifact version
  being added to a registry.
tags:
  - triggers
  - pipelines
---

# Trigger pipelines on a new artifact

You can trigger Harness pipelines in response to a new artifact version being added to a registry. For example, every time a new Docker image is pushed to your Docker Hub account, it triggers a CD pipeline that deploys it automatically.

An artifact trigger is a simple way to automate deployments for new builds.

{% hint style="info" %}
Currently, this feature is behind the feature flag `CD_TRIGGERS_REFACTOR`. Contact [Harness Support](mailto:support@harness.io) to enable it.
{% endhint %}

{% include "../../.gitbook/includes/__shared/shared/variables-not-supported.md" %}

***

### What you will learn in this topic <a href="#what-you-will-learn-in-this-topic" id="what-you-will-learn-in-this-topic"></a>

By the end of this topic, you will be able to:

* Identify the [supported artifact providers](trigger-on-a-new-artifact.md#supported-artifact-providers-for-artifact-triggers) for artifact triggers.
* [Create an artifact trigger](trigger-on-a-new-artifact.md#create-an-artifact-trigger) for your registry using the [Configuration](trigger-on-a-new-artifact.md#step-1-configuration), [Conditions](trigger-on-a-new-artifact.md#step-2-conditions), and [Pipeline Input](trigger-on-a-new-artifact.md#step-3-pipeline-input) steps.
* Configure a [multi-region artifact source](trigger-on-a-new-artifact.md#define-a-multi-region-artifact-source).

***

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

Before you create an artifact trigger, ensure you have the following:

* **A Harness CD pipeline**: An existing pipeline that includes an artifact in the stage's **Service Definition**.
* **CD pipeline familiarity**: Familiarity with Harness CD pipelines. Go to the [Kubernetes CD quickstart](https://app.gitbook.com/s/y1JhZ4oKIppwY7d5AhPj/use-continuous-delivery/deploy-services-on-different-platforms/kubernetes/kubernetes-cd-quickstart) to build a CD pipeline.

***

### How artifact triggers work <a href="#how-artifact-triggers-work" id="how-artifact-triggers-work"></a>

On New Artifact triggers listen to the registry where one or more of the artifacts in your pipeline are hosted. You can set conditions on the triggers, such as matching a Docker tag or label, or a traditional artifact build name or number.

{% hint style="info" %}
An artifact source does not need to be defined in the service definition for the trigger to work. The only possible failure scenario is during the initial collection of the artifact, within one minute of creating the trigger. For example, suppose the Docker registry contains 10 tags for a specific image and a trigger is created. In that case, the delegate's polling job retrieves all 10 tags and sends them to the manager, which does not initiate any pipelines. This is because running the pipeline for all 10 tags that were pushed before the trigger was created could leave the system in an undesirable state. However, when an 11th or any subsequent tag is pushed, the trigger executes and initiates the pipeline.
{% endhint %}

***

### Supported artifact providers for artifact triggers <a href="#supported-artifact-providers-for-artifact-triggers" id="supported-artifact-providers-for-artifact-triggers"></a>

You can use the following artifact providers to trigger pipelines:

* [Harness Artifact Registry](https://app.gitbook.com/s/GmEmgYs6IUcqNiV0OEgB/use-artifact-registry/manage-registries/ar-webhooks)
* ACR (Azure Container Registry)
* Amazon Machine Image (AMI)
* Amazon S3
* Artifactory
* Azure Artifacts
* Bamboo
* Custom
* Docker Registry
* ECR (Amazon Elastic Container Registry)
* GCE Image (Google Compute Engine Image)
* GCR (Google Container Registry)
* GitHub Package Registry
* Google Artifact Registry
* Google Cloud Storage
* Jenkins
* Nexus2
* Nexus3

Google Container Registry (GCR) is deprecated and was shut down on March 18, 2025. Migrate to Google Artifact Registry (GAR) instead. Go to [Google's official transition documentation](https://cloud.google.com/artifact-registry/docs/transition/transition-from-gcr) to review the migration path, and go to the [Harness GCR documentation](https://app.gitbook.com/s/y1JhZ4oKIppwY7d5AhPj/use-continuous-delivery/cd-building-blocks/services/artifact-sources#google-container-registry-gcr) to understand GCR support in Harness.

***

### Artifact trigger behavior <a href="#artifact-trigger-behavior" id="artifact-trigger-behavior"></a>

Review the following behavior and recommendations before you create an artifact trigger:

* **One artifact triggers deployment**: If more than one artifact is collected during the polling interval (one minute), only one deployment starts, and it uses the last artifact collected.
*   **All artifacts trigger deployment**: All artifacts collected during the polling interval trigger a deployment, with one deployment triggered for each artifact collected.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>To enable this feature, navigate to your Harness project, organization, or account <strong>Default Settings</strong>, select <strong>Pipeline</strong>, and then enable <strong>Execute Triggers With All Collected Artifacts or Manifests</strong>.</p><p>When this setting is enabled, a separate deployment is triggered for each artifact collected during the polling interval, which does not maintain the tag ordering. For example, if tags <code>v1.2.0</code> and <code>v1.1.0</code> are collected within the same polling window, you might see that <code>v1.2.0</code> is executed before <code>v1.1.0</code>.</p></div>
* **Trigger based on file name**: The trigger is executed based on _file names_ and not metadata changes.
* **Do not trigger on the latest tag**: Do not trigger on the **latest** tag of an artifact, such as a Docker image. With latest, Harness only has metadata, such as the tag name, which has not changed, so Harness does not know if anything has changed. The trigger is not executed.
* **Control access with repository RBAC**: In Harness, you can select who is able to create and use triggers, but you must use your repository's RBAC to control who can add the artifacts or initiate the events that start the Harness trigger.
*   **Verify a new trigger**: Whenever you create a trigger for the first time, Harness recommends submitting a tag or pushing an artifact to verify its functionality. This way, the trigger executes and the pipeline runs as expected when subsequent tags are pushed.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>When you link a Docker repository to a trigger, the trigger status remains <code>pending</code> until there are available tags. After the first artifact push, the trigger status changes to <code>success</code> because of new tags, but this alone does not activate the pipeline. <strong>The pipeline is only triggered after a second push to Docker.</strong></p></div>
* **Allow time for polling to start**: Whenever a trigger is created or updated, it takes about five to ten minutes for the polling job to start and for the trigger to be in a working state. Harness recommends that you wait five to ten minutes after a trigger is created or updated before you push the artifact.
* **Polling and disabled triggers**: Polling stops when you disable a trigger. Artifact polling restarts after you re-enable the trigger. Harness recommends that you submit a tag or push an artifact and verify the flow, because this is treated as a new polling job.
* **Use lexically sortable tags**: Due to a Docker API limitation, image build numbers and tags are always listed in lexical order. To ensure that executions are triggered with the image pushed last, a best practice is to create build numbers or tags that can be sorted lexically using their creation date. With this method, higher build numbers are assigned for later creation dates, which ensures that the image pushed last is used when more than one image is pushed over a short period, such as less than five minutes.

***

### Visual summary <a href="#visual-summary" id="visual-summary"></a>

The following five-minute video walks you through building an app from source code and pushing it to Docker Hub using Harness CIE, and then having an **On New Artifact Trigger** execute a CD pipeline to deploy the new app release automatically.

{% embed url="https://www.youtube.com/embed/nIPjsANiKRk" %}

***

### Artifact polling <a href="#artifact-polling" id="artifact-polling"></a>

After you create a trigger to listen for new artifacts, Harness polls for new artifacts continuously. Polling is immediate because Harness uses a perpetual task framework that constantly monitors for new builds and tags.

***

### Set the artifact tag to deploy <a href="#set-the-artifact-tag-to-deploy" id="set-the-artifact-tag-to-deploy"></a>

Set the artifact tag to control which artifact version the pipeline deploys when a trigger fires. When you add a Harness service to the CD stage, you set the artifact tag to use in **Artifacts Details**.

<figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-22.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Set the artifact **Tag** field to any of the following options:

* **Fixed value**: Deploy a specific [fixed value](../variables-and-expressions/runtime-inputs.md) tag, such as `2`. Harness deploys the artifact with that tag when the trigger executes the pipeline.
* **`<+trigger.artifact.build>`**: Deploy the artifact version that initiated the trigger.
* **`<+lastPublished.tag>`**: Deploy the last successful published artifact version.
* **`<+lastPublished.tag>.regex(regex)`**: Deploy the last successful published artifact version that matches a regex.

{% hint style="info" %}
The `lastPublished` tag returns the lexicographically last published tag for container image based artifact sources.
{% endhint %}

{% hint style="warning" %}
The raw artifact trigger payload uses the field name `artifactData` (for example, `artifactData.build`), but this is not a valid expression path. Use `<+trigger.artifact.build>` to reference the artifact version that started the trigger. `<+trigger.artifactData.build>` does not resolve.
{% endhint %}

You can also set the tag as a runtime input, and then use `<+trigger.artifact.build>` in the trigger's [pipeline input](trigger-on-a-new-artifact.md#step-3-pipeline-input) settings.

***

### Create an artifact trigger <a href="#create-an-artifact-trigger" id="create-an-artifact-trigger"></a>

Create an artifact trigger on a pipeline that already references an artifact, so a new artifact version starts a deployment. The trigger wizard walks you through three steps in order:

1. [**Configuration**](trigger-on-a-new-artifact.md#step-1-configuration): Name the trigger and define the artifact source to poll.
2. [**Conditions**](trigger-on-a-new-artifact.md#step-2-conditions): Set optional conditions that must match for the trigger to run the pipeline.
3. [**Pipeline Input**](trigger-on-a-new-artifact.md#step-3-pipeline-input): Provide the runtime inputs the pipeline needs when the trigger runs it.

To open the trigger wizard, do the following:

1.  Select a Harness pipeline that includes an artifact in the stage's **Service Definition**.

    <figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-24.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

    You reference an artifact in the stage's service definition in your manifests using the expression `<+artifact.image>`. Go to [Add container images as artifacts for Kubernetes deployments](https://app.gitbook.com/s/y1JhZ4oKIppwY7d5AhPj/use-continuous-delivery/deploy-services-on-different-platforms/kubernetes/cd-kubernetes-category/add-artifacts-for-kubernetes-deployments) to reference artifacts in your manifests.
2. Select **Triggers**.
3.  Click **New Trigger**.

    <figure><img src="../../.gitbook/assets/new-trigger-artifact.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
4. The **On New Artifact Trigger** options are listed under **Artifact**. Each of the **Artifact** options is described below.
5. Select the artifact registry where your artifact is hosted. If your artifact is hosted on Docker Hub and you select GCR, you cannot set up your trigger.

#### Step 1: Configuration <a href="#step-1-configuration" id="step-1-configuration"></a>

In **Configuration**, name the trigger and define the artifact source that Harness polls for new versions. Select the tab for your artifact registry.

{% tabs %}
{% tab title="Docker Registry Artifacts" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**. This is where you tell Harness which artifact repository to poll for changes.
3. Create or select the connector to connect Harness to the repository, and then click **Continue**. Go to the [Docker Registry connector settings reference](../connectors/artifact-repositories/docker-registry-connector-settings-reference.md) to configure Docker Registry connectors.
4.  In **Artifact Details**, enter the artifact for this trigger to listen for, and click **Submit**. For example, in Docker Hub, you might enter `library/nginx`. The artifact is now listed in the trigger.

    <figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-25.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
5.  Click **Continue**.

    In your Docker Registry connector, to connect to a public Docker registry like Docker Hub, use `https://registry.hub.docker.com/v2/`. To connect to a private Docker registry, use `https://index.docker.io/v2/`.
{% endtab %}

{% tab title="GCR Artifacts" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. Create or select the GCP connector to connect Harness to GCR, and then click **Continue**. Go to [Add a Google Cloud Platform (GCP) connector](../connectors/cloud-providers/connect-to-google-cloud-platform-gcp.md) to configure GCP connectors.
4.  In **Artifact Details**, in **GCR Registry URL**, select the location of the registry, listed as **Hostname** in GCR.

    <figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-26.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
5.  In **Image Path**, enter the artifact for this trigger to listen for. You can click the copy button in GCR and then paste the path into Harness.

    <figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-27.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
6. Click **Submit**, and then click **Continue**.
{% endtab %}

{% tab title="ECR Artifacts" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. In **Artifact Repository**, create or select the AWS connector to connect Harness to ECR, and then click **Continue**. Go to the [AWS connector settings reference](../connectors/cloud-providers/aws-connector-settings-reference.md) to configure AWS connectors.
4. In **Artifact Location**, in **Region**, select the region for the ECR service you are using.
5. (Optional) In **Registry ID**, enter the AWS account ID of the ECR registry you want to use. This field is useful when the AWS connector can access AWS accounts other than the one it is configured with. If you do not specify a registry ID, Harness uses the default registry associated with the AWS account.
6. In **Image Path**, enter the path to the repo and image. You can copy the URI value from the repo in ECR. For example, `public.ecr.aws/l7w9l6a8/todolist` (public repo) or `085111111113.dkr.ecr.us-west-2.amazonaws.com/todolist` (private repo).
7. Click **Continue**.
{% endtab %}

{% tab title="AWS S3" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. Create or select the AWS connector to connect Harness to S3, and then click **Continue**. Go to the [AWS connector settings reference](../connectors/cloud-providers/aws-connector-settings-reference.md) to configure AWS connectors.
4. In **Artifact Details**, in **Region**, select the region for the S3 service you are using. While S3 is regionless, Harness needs a region for the S3 API.
5. In **Bucket Name**, enter the S3 bucket name.
6.  In **File Path Regex**, enter a regex like `todolist*.zip`. The expression must either contain a `*` or end with `/`.

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>Use <code>*</code> for regex matching, not <code>.*</code>. For example, <code>*.tgz</code> or <code>todolist-v*.zip</code>.</p></div>
7. Click **Continue**.
{% endtab %}

{% tab title="Artifactory" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. Create or select the Artifactory connector to connect Harness to Artifactory, and then click **Continue**. Go to the [Artifactory connector settings reference](../connectors/artifact-repositories/artifactory-connector-settings-reference.md) to configure Artifactory connectors.
4. In **Artifact Details**, in **Repository Format**, select **Generic** or **Docker**.
   1. Generic:
      1. **Repository**: enter the **Name** of the repo.
      2. **Artifact Directory**: enter the path to the **Directory** that is inside the repo.
   2. Docker:
      1. **Repository**: enter the **Name** of the repo.
      2. **Artifact/Image Path**: enter the path to the **Artifact/Image** that is inside the repo.
      3. **Repository URL (optional)**: enter the **URL to file**.
5. Click **Continue**.
{% endtab %}

{% tab title="ACR" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. Create or select the Azure connector to connect Harness to ACR, and then click **Continue**. Go to [Add a Microsoft Azure Cloud connector](../connectors/cloud-providers/add-a-microsoft-azure-connector.md) to configure Azure connectors.
4. In **Artifact Details**, in **Subscription Id**, select the Subscription Id from the ACR registry.
5. In **Registry**, select the registry you want to use.
6. In **Repository**, select the repository to use.
7. Click **Continue**.
{% endtab %}

{% tab title="Bamboo" %}
1. In **Configuration**, in **Name**, enter a name for the trigger.
2. In **Listen on New Artifact**, select **Define Artifact Source**.
3. Create or select the Bamboo connector to connect Harness to Bamboo, and then click **Continue**.
4. In **Artifact Details**, specify the plan name, artifact paths, and builds to monitor.
5. Click **Continue**.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
To trigger the pipeline based on the same artifact version being available across regions, go to [Define a multi-region artifact source](trigger-on-a-new-artifact.md#define-a-multi-region-artifact-source) instead of a single artifact source.
{% endhint %}

When you are done, click **Continue** to move to **Conditions**.

#### Step 2: Conditions <a href="#step-2-conditions" id="step-2-conditions"></a>

In **Conditions**, specify the conditions that must be met for the trigger to run the pipeline. For example, run the pipeline only when an artifact tag, label, filename, or build matches a certain value or pattern. Conditions are optional.

<figure><img src="../../.gitbook/assets/event-metadata-conditions.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

An artifact trigger supports the following condition types:

* **Event Condition**: Match against the incoming **Artifact Build**. Select an operator and enter a value or pattern to match the artifact build that fired the trigger.
* **Metadata Conditions**: Match against one or more artifact metadata attributes, such as the image, tag, or SHA. Go to [Set metadata conditions](trigger-on-a-new-artifact.md#set-metadata-conditions) for the supported attributes.
* **JEXL Condition**: Enter a JEXL expression for advanced logic that combines multiple attributes or values.

When the trigger fires, Harness evaluates every condition you set. The pipeline runs only when all of the conditions match.

**Regex and wildcards**

You can use wildcards in the condition's value, and you can select **Regex**.

For example, if the build is `todolist-v2.0`:

* With regex selected, the regex `todolist-v\d.\d` matches.

If the regex expression does not result in a match, Harness ignores the value.

Harness supports standard Java regex. For example, if regex is enabled and the intent is to match any branch, the wildcard should be `.*` instead of simply a wildcard `*`. To match all of the files that end in `-DEV.tar`, enter `.*-DEV\.tar`.

**Set metadata conditions**

On New Artifact triggers support conditions based on artifact metadata expressions. You can define conditions based on metadata apart from the artifact build and JEXL conditions.

To configure a condition based on artifact metadata, do the following:

1. In **Metadata Conditions**, click **Add**.
2. In **Attribute**, enter a metadata expression, such as `<+trigger.artifact.metadata.field>`.
3. Select an operator and enter a value to match against the metadata attribute when the expression is resolved.

When the trigger is executed, the metadata condition is evaluated and, if the condition matches, the pipeline is executed.

The following are the artifact metadata expressions you can use:

{% tabs %}
{% tab title="Docker registry" %}
You can use the following expressions:

```bash
<+pipeline.stages.DS.spec.artifacts.primary.metadata.image>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.tag>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.SHAV2>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.SHA>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.url>
<+pipeline.stages.DS.spec.artifacts.primary.dockerConfigJsonSecret>
```
{% endtab %}

{% tab title="ECR" %}
You can use the following expressions:

```bash
<+pipeline.stages.DS.spec.artifacts.primary.metadata.image>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.tag>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.SHAV2>
<+pipeline.stages.DS.spec.artifacts.primary.metadata.SHA>
<+pipeline.stages.DS.spec.artifacts.primary.dockerConfigJsonSecret>
```
{% endtab %}

{% tab title="ACR" %}
You can use the following expressions:

```bash
<+pipeline.stages.s1.spec.artifacts.primary.metadata.image>
<+pipeline.stages.s1.spec.artifacts.primary.metadata.registryHostname>
<+pipeline.stages.s1.spec.artifacts.primary.metadata.tag>
<+pipeline.stages.s1.spec.artifacts.primary.metadata.SHAV2>
<+pipeline.stages.s1.spec.artifacts.primary.metadata.SHA>
<+pipeline.stages.s1.spec.artifacts.primary.metadata.url>
```
{% endtab %}

{% tab title="GAR" %}
The following are the expressions for Google Artifact Registry (GAR):

```bash
<+pipeline.stages.firstS.spec.artifacts.primary.metadata.image>
<+pipeline.stages.firstS.spec.artifacts.primary.metadata.registryHostname>
<+pipeline.stages.firstS.spec.artifacts.primary.metadata.SHAV2>
<+pipeline.stages.firstS.spec.artifacts.primary.metadata.SHA>
```
{% endtab %}

{% tab title="Artifactory" %}
You can use the following expressions:

```bash
<+pipeline.stages.tas_0.spec.artifacts.primary.metadata.fileName>
<+pipeline.stages.tas_0.spec.artifacts.primary.metadata.url>
```
{% endtab %}

{% tab title="Jenkins" %}
You can use the following expressions:

```bash
<+pipeline.stages.SSH_Jenkins_ArtifactSource.spec.artifacts.primary.metadata.url>
```
{% endtab %}

{% tab title="Nexus 2" %}
You can use the following expressions:

```bash
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.fileName>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.package>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.repositoryName>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.version>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.url>
```
{% endtab %}

{% tab title="Nexus 3" %}
You can use the following expressions:

```bash
<+pipeline.stages.SSH_Nexus3_Maven.spec.artifacts.primary.metadata.extension>
<+pipeline.stages.SSH_Nexus3_Maven.spec.artifacts.primary.metadata.fileName>
<+pipeline.stages.SSH_Nexus3_Maven.spec.artifacts.primary.metadata.imagePath>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.repositoryName>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.version>
<+pipeline.stages.SSH_Nexus2_NPM.spec.artifacts.primary.metadata.url>
<+pipeline.stages.SSH_Nexus3_Maven.spec.artifacts.primary.metadata.artifactId>
<+pipeline.stages.SSH_Nexus3_Maven.spec.artifacts.primary.metadata.groupId>
```
{% endtab %}
{% endtabs %}

***

#### Step 3: Pipeline Input <a href="#step-3-pipeline-input" id="step-3-pipeline-input"></a>

In **Pipeline Input**, provide the runtime inputs the pipeline needs when the trigger runs it. If the pipeline has no runtime inputs, Harness displays **No Runtime Inputs**.

<figure><img src="../../.gitbook/assets/pipeline-input-artifact.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

If your pipeline uses [input sets](../pipelines/input-sets.md), you can select the input set to use when the trigger executes the pipeline.

{% hint style="info" %}
When you configure pipeline inputs for a trigger, you can use either an **input set** or provide **runtime values** directly, but not both at the same time. If you select an input set, any fields not covered by the input set use their default values. To override specific values from an input set while keeping the rest, use the [override YAML approach](customize_trigger_input_configuration_using_override_yaml.md) in the trigger configuration.
{% endhint %}

You can reference trigger event payload values in the pipeline input using `<+eventPayload.[path-to-key-name]>`. For trigger header values, use `<+trigger.header[key name]>`.

When you are done, click **Create Trigger**.

**Customize trigger input configuration using override YAML**

To use an input set for both trigger and manual runs, override input parameters in the trigger `inputYAML` configuration. This provides the flexibility to modify a specific parameter within the associated `Input Set`. Go to [Customize trigger input configuration using override YAML](customize_trigger_input_configuration_using_override_yaml.md) to override input parameters.

***

### Define a multi-region artifact source <a href="#define-a-multi-region-artifact-source" id="define-a-multi-region-artifact-source"></a>

Configure a multi-region artifact source when the same artifact version is available across regions and you want the pipeline to trigger based on availability in different regions. This is an alternative to the single artifact source you define in [Step 1: Configuration](trigger-on-a-new-artifact.md#step-1-configuration).

When artifact repositories such as Google Artifact Registry (GAR) are enabled with multi-region support, artifacts of the same version are available across different regions for easy consumption. Each region can have similar artifacts. This support enables the configuration of Harness triggers using artifacts from multiple regions.

In On New Artifact triggers, you can configure the regions and conditions associated with the artifact across regions. This enables the pipeline to be triggered based on the availability of artifacts in different regions.

To configure multi-region for the artifact, do the following:

1. In your pipeline, select **Triggers**.
2. Create an **On New Artifact** trigger for your artifact registry.
3. In **Configuration**, in the **Listen on New Artifact** section, add the primary artifact. This is the region where the artifact is first available, and the Harness connector you use must point to that region.
4.  After you add the primary artifact, select **Define Multi Region Artifact Source** and add artifacts corresponding to the other regions. Add as many regions as needed for the trigger.

    <figure><img src="../../.gitbook/assets/multi-region-listen-on-new-artifact.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

    *   In **Artifact Repository**, select or create the connector, and then click **Continue**. In **Artifact Location**, set the **Repository Format** and **Repository**, and then click **Submit**.

        <figure><img src="../../.gitbook/assets/multi-region-artifact-details.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
5. When you are done, click **Continue** to move to **Conditions**.
6.  Select the conditions required for the artifacts across different regions.

    When the artifact version is available across different regions, the condition is evaluated for all the artifacts and the pipeline is triggered.
7. Complete the trigger setup.

***

### Enable or disable a trigger <a href="#enable-or-disable-a-trigger" id="enable-or-disable-a-trigger"></a>

Use the enabled toggle to enable or disable a trigger:

<figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-28.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

### Reuse trigger YAML to create new triggers <a href="#reuse-trigger-yaml-to-create-new-triggers" id="reuse-trigger-yaml-to-create-new-triggers"></a>

Reuse triggers by copying and pasting trigger YAML. This is helpful when you have advanced conditions you do not want to set up each time.

<figure><img src="../../.gitbook/assets/trigger-on-a-new-artifact-29.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
Trigger artifact expressions used in a pipeline are resolved when you rerun a pipeline that was activated by a trigger.
{% endhint %}

***

### Related articles <a href="#related-articles" id="related-articles"></a>

* [Schedule pipelines using triggers](schedule-pipelines-using-cron-triggers.md): Schedule pipeline executions with cron triggers.
* [Trigger pipelines using Git events](triggering-pipelines.md): Run pipelines in response to Git events.

{% @harness-feedback/feedback module="harness-ai" pagePath="harness-ai/use-harness-platform/triggers/trigger-on-a-new-artifact" %}
