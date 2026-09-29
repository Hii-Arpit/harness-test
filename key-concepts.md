---
description: Key Concepts
---


# Key Concepts

{% embed url="https://app.tango.us/app/embed/89164540-a07f-4900-bca7-b303fbb37154?skipCover=false&defaultListView=false&skipBranding=false&makeViewOnly=true&hideAuthorAndDetails=true" %}
Perspective Overview in Harness CACM
{% endembed %}

## Overview <a href="#overview" id="overview"></a>

Perspectives in Harness CACM provide powerful cost analysis capabilities through customizable views of your cloud spending data. This guide covers the key concepts and features available in Perspectives.

## Perspective Drilldown <a href="#perspective-drilldown" id="perspective-drilldown"></a>

<figure><img src="../../.gitbook/assets/total-cost.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Total Cost**: The total cost of the resources in the Perspective.
* **Budget**: The budget for the resources in the Perspective.
* **Forecasted Cost**: The forecasted cost of the resources in the Perspective.
* **Recommendations**: The recommendations for the resources in the Perspective.

### Group By <a href="#group-by" id="group-by"></a>

<figure><img src="../../.gitbook/assets/group-by.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

You can create a Perspective for your resources using rules and filters. The filters are used to group the resources. The following are the supported filters:

* **Cost Categories**: You can create a perspective by filtering based on the cost categories you have created. To create cost categories, see [Use Cost Categories](../cost-categories/cost-categories.md).
* **Region**: Each AWS, GCP, or Azure region you're currently running services in.
* **Product**: Each of your active products with its cloud costs.
* **Cloud Provider**: Filter and group costs by the cloud service provider (AWS, GCP, Azure, or Kubernetes clusters) to analyze spending across different cloud platforms.
* **Label**: Each label that you assign to your AWS resources. You can select a label name to get more granular details of your label. For more information, go to [Tagging your AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html). For tags to appear in the Perspective, you must activate the user-defined cost allocation tags in the AWS Billing and Cost Management console. For more information, go to [Activating User-Defined Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html). CACM updates the tag keys as follows:
  * For the user-defined tags, `user_` prefix is added.
  * For the AWS system tags, `aws_` prefix is added.
  * The characters that do not follow regex `[a-zA-Z0-9_]` are changed to `_`.
  * The tags are case-sensitive. If the tags are specified as `UserName` and `username`, then the number suffix `_<Number>`is added to the tag. For example, `UserName` and `username_1`.
* **Label V2**: Preserves the original structure from AWS similar to how GCP, Azure and Cluster tags are stored. See [Understanding the Difference: Label vs. Label V2](key-concepts.md#understanding-the-difference-label-vs-label-v2-and-migration) and [Migrate from Label to Label V2](key-concepts.md#migration-from-label-to-label-v2).

**Grouping Options by Data Source**

CACM provides various grouping options to analyze your cloud costs based on different dimensions. Select the appropriate tab to view available grouping options for each cloud provider, container platform, or external data source.

{% tabs %}
{% tab title="AWS" %}
**AWS Grouping Options**

CACM allows you to view your AWS costs at a glance, understand what is costing the most, and analyze cost trends across all your Amazon Web Services:

| Grouping Option    | Description                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Account**        | Cost by AWS account connected via Harness AWS Cloud Provider, showing account name and ID                                                                                |
| **Billing Entity** | Distinguishes between AWS Marketplace transactions and other AWS service purchases ([Learn more](https://docs.aws.amazon.com/cur/latest/userguide/billing-columns.html)) |
| **Instance Type**  | Cost by [Amazon EC2 instance type](https://aws.amazon.com/ec2/instance-types/) (e.g., t2.micro, m5.large)                                                                |
| **Line Item Type** | Cost by charge type (Usage, Tax, Credit, etc.) ([Learn more](https://docs.aws.amazon.com/cur/latest/userguide/Lineitem-columns.html))                                    |
| **Payer Account**  | Cost by AWS account that pays for member accounts in an AWS Organization                                                                                                 |
| **Service**        | Cost by AWS service (EC2, S3, RDS, etc.)                                                                                                                                 |
| **Usage Type**     | Cost by specific resource usage measurement (e.g., BoxUsage:t2.micro(Hrs) for EC2 t2.micro instance hours)                                                               |
{% endtab %}

{% tab title="Azure" %}
**Azure Grouping Options**

Analyze your Microsoft Azure costs across services, resource groups, and other dimensions:

| Grouping Option          | Description                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------- |
| **Benefit Name**         | Cost by benefit applied to resources (Enterprise Agreement discounts, Azure Hybrid Benefit, etc.) |
| **Billing Account ID**   | Cost by unique billing account identifier                                                         |
| **Billing Account Name** | Cost by billing account name                                                                      |
| **Charge Type**          | Cost by charge type (Usage, Purchase, Refund, etc.)                                               |
| **Frequency**            | Cost by charge frequency (OneTime, Recurring, UsageBased)                                         |
| **Instance ID**          | Cost by specific resource instance identifier                                                     |
| **Meter**                | Cost by usage meter (Compute Hours, IP Address Hours, Data Transfer, etc.)                        |
| **Meter Category**       | Cost by meter category (Cloud Services, Networking, etc.)                                         |
| **Meter Subcategory**    | Cost by meter subcategory (A6 Cloud Services, ExpressRoute, etc.)                                 |
| **Pricing Model**        | Cost by pricing structure (Pay-as-you-go, Reserved Instance, etc.)                                |
| **Publisher Name**       | Cost by Marketplace service publisher                                                             |
| **Publisher Type**       | Cost by publisher type (Microsoft/Azure, Marketplace, AWS)                                        |
| **Reservation ID**       | Cost by reservation instance identifier                                                           |
| **Reservation Name**     | Cost by reservation instance name                                                                 |
| **Resource**             | Cost by specific Azure resource                                                                   |
| **Resource GUID**        | Cost by resource unique identifier                                                                |
| **Resource Group Name**  | Cost by resource group                                                                            |
| **Resource Name**        | Cost by resource name                                                                             |
| **Resource Type**        | Cost by resource type (Virtual Machine, Storage Account, App Service, etc.)                       |
| **Service Name**         | Cost by Azure service (Virtual Machines, App Service, DNS, etc.)                                  |
| **Service Tier**         | Cost by service tier (VMs, Dv3, Dsv3, etc.)                                                       |
| **Subscription ID**      | Cost by subscription identifier                                                                   |
| **Subscription Name**    | Cost by subscription name                                                                         |
{% endtab %}

{% tab title="GCP" %}
**GCP Grouping Options**

Analyze your Google Cloud Platform costs across products, projects, and other dimensions:

| Grouping Option     | Description                                                                                            |
| ------------------- | ------------------------------------------------------------------------------------------------------ |
| **Billing Account** | Cost by billing account, allowing tracking across multiple linked projects                             |
| **Invoice Month**   | Cost by billing period                                                                                 |
| **Product**         | Cost by GCP product (Compute Engine, Cloud Storage, BigQuery, etc.)                                    |
| **Project**         | Cost by GCP project                                                                                    |
| **SKU**             | Cost by specific [SKU](https://cloud.google.com/skus), representing the billable unit for GCP services |
{% endtab %}

{% tab title="External Data" %}
**External Data Grouping Options**

Analyze costs from external data sources that you've integrated with CCM:

| Grouping Option          | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| **Account ID**           | Cost by account identifier from external data sources    |
| **Account Name**         | Cost by account name from external data sources          |
| **Billing Account ID**   | Cost by billing account identifier from external systems |
| **Billing Account Name** | Cost by billing account name from external systems       |
| **Provider Name**        | Cost by provider or vendor name from external data       |
| **Resource**             | Cost by specific resource from external data sources     |
| **SKU**                  | Cost by SKU or product identifier from external systems  |
| **Data Source**          | Cost by external data source name or identifier          |
| **Category**             | Cost by custom categories defined in your external data  |
| **Cost Center**          | Cost by organizational cost center from external systems |
| **Department**           | Cost by department or business unit from external data   |
| **Project**              | Cost by project identifier from external systems         |
| **Team**                 | Cost by team or group from external data sources         |
| **Custom Field 1**       | Cost by first custom field defined in external data      |
| **Custom Field 2**       | Cost by second custom field defined in external data     |
| **Custom Field 3**       | Cost by third custom field defined in external data      |
| **Custom Field 4**       | Cost by fourth custom field defined in external data     |
| **Custom Field 5**       | Cost by fifth custom field defined in external data      |
{% endtab %}

{% tab title="Kubernetes &amp; ECS" %}
**Kubernetes & Container Grouping Options**

Analyze costs for your Kubernetes clusters and ECS environments with these grouping options:

| Grouping Option        | Description                                                                                                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cluster Name**       | Total cost, cost trend, idle cost, unallocated cost, and efficiency score for each cluster name                                                                         |
| **Cluster Type**       | Cost by cluster type (e.g., ECS, Kubernetes)                                                                                                                            |
| **Namespace**          | Cost of each Kubernetes namespace in the cluster (not applicable to ECS clusters)                                                                                       |
| **Namespace ID**       | Cost of each Kubernetes namespace ID in the cluster                                                                                                                     |
| **Workload**           | Cost of each Kubernetes workload or ECS service, with workload types identified as Kubernetes pods or ECS tasks                                                         |
| **Workload ID**        | Cost of each Kubernetes workload ID                                                                                                                                     |
| **Node**               | Cost of each Kubernetes node or ECS instance                                                                                                                            |
| **Storage**            | Cost of persistent volumes in your Kubernetes cluster ([Learn more about Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/))          |
| **Application**        | Sum of your Harness Application costs                                                                                                                                   |
| **Environment**        | Cost of cloud platform infrastructures grouped by environment (Dev, QA, Stage, Production, etc.)                                                                        |
| **Service**            | Cost of your microservices and applications                                                                                                                             |
| **Cloud Provider**     | Cost of your cloud platforms (AWS, Kubernetes, etc.)                                                                                                                    |
| **ECS Service**        | Cost of ECS services running task definition instances ([Learn more about ECS Services](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html)) |
| **ECS Launch Type**    | Cost by launch type (Fargate or EC2)                                                                                                                                    |
| **ECS Launch Type ID** | Cost by unique identifier for an Amazon ECS launch type                                                                                                                 |
| **ECS Service ID**     | Cost by unique identifier for an ECS service                                                                                                                            |
| **ECS Task**           | Cost of each ECS task (smallest deployable unit in Amazon ECS)                                                                                                          |
| **ECS Task ID**        | Cost by unique identifier for an ECS task                                                                                                                               |
{% endtab %}
{% endtabs %}

### Preferences <a href="#preferences" id="preferences"></a>

<figure><img src="../../.gitbook/assets/perspectives-preferences.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

**General Preferences**

* **Show Others**: The default perspective graph displays only the top 12 cost items. Enable this option to group all remaining costs into an "Others" category, ensuring you see your complete spending picture.
* **Show Anomalies**: Highlight unusual spending patterns or sudden cost changes in your visualizations. This feature makes it easy to spot potential issues or unexpected charges that may require investigation. The number of anomalies are shown on the Group By graph in a red triangle.

<figure><img src="../../.gitbook/assets/anomalies.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Show Negative Cost**: Displays instances where discounts exceed the actual billing amount, resulting in negative cost values in your reports. Displays the negative cost with a dotted red bar and labels it as NegativeCost in the legend. To view it, select "Group By" as None because in other Group Bys, it might not appear in the top 12 entries.
* **Show Unallocated costs on clusters**: In certain graphs, you may come across an item labeled as Unallocated. This entry is included to provide a comprehensive view of all costs. When you examine the Total Cost in the perspective, it encompasses the costs of all items, including the unallocated cost. This option is available only in perspectives with cluster rules. The Show "unallocated" costs on clusters option is only available in the chart when the Group By is using Cluster and the following options are selected:
  * Namespace
  * Namespace ID
  * Workload
  * Workload ID
  * ECS Task
  * ECS Task ID
  * ECS Service ID
  * ECS Service
  * ECS Launch Type ID
  * ECS Launch Type

<figure><img src="../../.gitbook/assets/cluster-cost.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
When preferences are selected in a Perspective on the perspective overview page, those settings are saved automatically. Upon returning to the same Perspective, the previously selected preferences are reapplied. Once the user logs out, all view preferences stored in the cache will be cleared. The preferences include (but not limited to):

* Group By selection
* Filters applied
* Cost preference
* Cost granularity
{% endhint %}

**Cloud-Based Preferences**

* [AWS Preferences](creating-a-perspective.md)
* [GCP Preferences](creating-a-perspective.md)
* [Azure Preferences](creating-a-perspective.md)

{% hint style="info" %}
**UNDERSTANDING "OTHERS" VS "NO \[GROUPBY]" CATEGORIES**

When working with Perspectives that include multiple data sources, you'll see two different grouping behaviors:

**"Others" Category**

* Shows when you have **more than 12 cost items** in a single data source
* Groups the smaller cost items together for cleaner visualization
* **Example**: If you have 20 AWS services, the top 12 show individually and the rest appear as "Others"

**"No \[GroupBy]" Category**

* Appears when you group by a field that **doesn't exist in all your data sources**
* Shows costs from data sources that don't have the selected grouping field

**Real-World Examples**

**Scenario 1**: You have AWS + GCP data and group by "AWS->Account"

* **AWS costs** → Show by individual account names
* **GCP costs** → Show as "No Account" (because GCP doesn't have AWS accounts)

**Scenario 2**: You have AWS + GCP data and group by "GCP->Project"

* **GCP costs** → Show by individual project names
* **AWS costs** → Show as "No Project" (because AWS doesn't use GCP projects)

**🔍 How to Filter "No \[GroupBy]" Items**

To view only the costs that appear as "No \[GroupBy]", use the **IS NULL** filter:

* **Filter**: `AWS > Account` IS NULL → Shows only non-AWS costs
* **Filter**: `GCP > Project` IS NULL → Shows only non-GCP costs
{% endhint %}

## Features behind Feature Flag

{% hint style="info" %}
**Behind a Feature Flag**

The features listed below are behind a feature flag. Contact [Harness Support](mailto:support@harness.io) to enable a feature for your account.
{% endhint %}

### Dynamic Perspective Reports <a href="#dynamic-perspective-reports" id="dynamic-perspective-reports"></a>

**What are Dynamic Perspective Reports?**

Dynamic Perspective Reports are a new capability that allows you to generate, schedule, and manage cost reports directly from your Perspectives. Create reports from your perspectives to bookmark specific filter and grouping configurations. No need to rebuild the same view repeatedly just save it once and access it anytime. Reports dynamically include all relevant data columns based on your selected grouping criteria and filters. This ensures consistency between your interactive perspective view and exported reports.

{% tabs %}
{% tab title="Create Reports" %}
{% embed url="https://app.tango.us/app/embed/9d694765-9542-47a8-a2fd-a23a2c01e2ca" %}
Perspective Overview in Harness CACM
{% endembed %}

1. Navigate to the specific Perspective you wish to create a report for.
2. Click the **Download/Save as Report** button in the upper-right corner of the Perspective view

**Report Details**

* **Name**: Provide a name for your report
* **Group By**: Select how data should be organized using [available grouping options](key-concepts.md#grouping-options-by-data-source)
* **Time Period**: Choose from Last 7 Days, Last 30 Days, Last Month, or This Month
* **Granularity**: Select Daily, Weekly, or Monthly data points
* **Filters**: Apply specific [filters](key-concepts.md#group-by) to focus your report
* **Data Columns**: Customize which metrics and dimensions appear in your report. The system automatically suggests relevant columns based on your Group By selection, but you can add or remove specific data points to tailor the report to your requirements.
* **Export Rows up to**: Set maximum number of data rows to include
* **Exclude rows with cost below**: Optionally exclude rows below a specified cost value

**Delivery Options:**

* **Scheduled Delivery:** Set specific date and time for automated delivery and frequency (Daily, Weekly, Monthly, Quarterly, Yearly). You can add up to 50 recipient email addresses (comma-separated)
* **Immediate Download:** Download Perspective Chart data or Table data as CSV

{% hint style="warning" %}
**IMPORTANT**

* When the Dynamic Perspective Reports feature flag is enabled for your account, CACM will disable the legacy report creation method from the Perspective Creation workflow.

<figure><img src="../../.gitbook/assets/enabled-ff.png" alt="" width="80%"><figcaption></figcaption></figure>

* For previously created reports where **Group By settings, time range, or granularity were not available for reports**, CACM automatically takes these parameters from the perspective. In these cases, the maximum export row limit defaults to 10,000 rows.
{% endhint %}
{% endtab %}

{% tab title="View Saved Reports" %}
<figure><img src="../../.gitbook/assets/saved-reports.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

To view and manage all your saved reports: Navigate to **Cloud Costs** > **Perspectives** > **Saved Reports**

The Saved Reports page provides a comprehensive view of all your configured reports with the following options:

* **Report Creation**: Create new reports by clicking **+New Report** > **Perspective**
* **Report Management**: View information for each report including:
  * Report name and scheduled delivery frequency
  * Associated perspective
  * Time period selected for data
  * Creation and modification metadata
* **Report Actions**: You can:
  * Edit or Delete a report.
  * Subscribe or unsubscribe from scheduled deliveries.
{% endtab %}
{% endtabs %}

***

### Dynamic Cost Categories Toggle <a href="#dynamic-cost-categories-toggle" id="dynamic-cost-categories-toggle"></a>

**Feature Flag Name: `CCM_COST_CATEGORY_STAMPED_DATA`**

<figure><img src="../../.gitbook/assets/dynamic-toggle.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

The Dynamic toggle on the Perspective page gives you control over how cost category rules are applied to your cost data. It lets you balance real-time accuracy with faster performance.

**Internal Working:**

<figure><img src="../../.gitbook/assets/dynamic-toggle.gif" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

**If Dynamic ON (Runtime Mode):**

* Perspectives apply **cost category rules as soon as the page loads (dynamically)**.
* Any recent changes to cost category rules (even from a few minutes ago) are reflected immediately.
* ⏱️ Load times may be slower since rules are processed at runtime.

**If Dynamic OFF (Stored Data Mode):**

* Harness CACM evaluates cost category rules during the daily data ingestion process and persists the results in a dedicated dataset. This optimized dataset contains pre-computed rule evaluations for the current month's cost data.
* When Dynamic Toggle is OFF, Perspectives use **cost category rules from the stored dataset**.
* This ensures ⚡ **faster performance** since stored data is used without any computations at runtime.
* Note that if in case, CACM's daily process of updating the dataset is still running, **stored data is not available and only Dynamic ON (runtime calculation) is available** and a message will be displayed on the UI to inform.

<figure><img src="../../.gitbook/assets/updation-in-process.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
* **Historical Data**: CACM updates cost category rules **daily for the current month only**. For previous months with Dynamic OFF, Perspectives use the rules that existed when that month's data was ingested. To apply new rules to historical data, contact support to request a backfill.
* **New Cost Categories**: When you create a new cost category and use it in a Perspective with Dynamic OFF (stored data mode), no data will be displayed initially. This occurs because the new cost category hasn't been processed by CACM's daily job yet. You'll need to either switch to Dynamic ON temporarily or wait for up to 24 hours for the new cost category data to appear.
{% endhint %}

***

## Label Migration: Label vs. Label V2 <a href="#label-migration-label-vs-label-v2" id="label-migration-label-vs-label-v2"></a>

Harness CACM is transitioning from the traditional Label system to the enhanced Label V2 system. Support for the legacy Label system will be discontinued in the coming months.

* **Label (Legacy)**: Normalizes AWS tags. GCP, Azure and Clusters tags are not normalized.
* **Label V2 (New)**: Preserves the original structure from AWS similar to how GCP, Azure and Cluster tags are stored.

### Key Benefits of Label V2: <a href="#key-benefits-of-label-v2" id="key-benefits-of-label-v2"></a>

* Original tags: Displays your original cloud tag keys exactly as they appear in AWS, Azure, or GCP
* Improved Performance: Enhanced data processing and query performance

After Label V2, AWS labels are stored as-is without any normalization.

{% hint style="info" %}
**Account-level tags** appear only under **Label V2**. Because Label V2 preserves the original tag structure, account-level tags such as AWS `accountTag/`, GCP `projectLabels/`, and Azure inherited subscription tags show up as keys you can group by or filter on. They do not exist under the legacy Label system. Go to [Account tags in CACM](../../use-cacm/account-level-tags.md) to understand how each provider surfaces them.
{% endhint %}

### Who Needs to Migrate? <a href="#who-needs-to-migrate" id="who-needs-to-migrate"></a>

**✅ Migration Required**

Label V2 will replace the current Labels in the next release. Harness CACM will automatically migrate your existing rules. However, if your scripts reference Labels in Perspectives or CCs, you’ll need to [update them manually](key-concepts.md#how-to-migrate) to use Label V2. **Existing Labels will also continue to work without interruption**

**🆕 No Migration Needed**

If you're a new user or haven't used Labels in your Perspectives, simply use **Label V2** for all new configurations.

### How to Migrate <a href="#how-to-migrate" id="how-to-migrate"></a>

{% tabs %}
{% tab title="Via UI" %}
* Identify affected components: Review all Perspectives that use Label-based grouping or filtering
* Update each component: Edit each Perspective. Locate all instances where you've defined rules, filters, or grouping using AWS Labels. Change the selection from "Label" to "Label V2". Save your changes
* Verify your updates: After updating the Perspective, confirm that your cost data appears correctly. Ensure all previously configured Label-based filters work as expected

{% embed url="https://app.tango.us/app/embed/44d091fd-3177-44a1-b575-1a5a8febf36d" %}
{% endtab %}

{% tab title="Via API" %}
**Label (Original Format)** normalizes AWS tags as follows:

* User-defined tags: Adds `user_` prefix (e.g., `environment` becomes `user_environment`)
* AWS system tags: Adds `aws_` prefix (e.g., `aws:createdBy` becomes `aws_createdBy`)
* Special characters: Characters not matching `[a-zA-Z0-9_]` are replaced with `_`
* Case sensitivity: For duplicate tag names with different cases (e.g., `UserName` and `username`), a numeric suffix is added (`UserName` and `username_1`)

**Label V2** preserves the original tag structure without these modifications

So, when migrating from Label to Label V2 in your API calls:

1. Change the `identifier` from `LABEL` to `LABEL_V2`
2. Change the `identifierName` from `Label` to `Label V2`
3. For AWS, identify all Labels that start with `user_` or `aws_` prefixes and replace them with the actual tag key (without the prefix). For other cloud providers, no Label prefix changes are needed.

Here's how the API request format changes:

{% tabs %}
{% tab title="AWS Users" %}
Earlier every request had the Label field as:

```json
                {
                    field": {
                        "fieldId": "labels.value",
                        "fieldName": "user_key1",
                        "identifier": "LABEL",
                        "identifierName": "Label"
                    },
                    "operator": "IN",
                    "values": [
                        "value1"
                    ]
                } 
```

Now the request has the Label V2 field as:

```json

                {
                    field": {
                        "fieldId": "labels.value",
                        "fieldName": "Key1",
                        "identifier": "LABEL_V2",
                        "identifierName": "Label V2"
                    },
                    "operator": "IN",
                    "values": [
                        "value1"
                    ]
                }
```

Similarly, for `fieldId`:

Earlier:

```json
 "idFilter": {
                    "field": {
                        "fieldId": "labels.key",
                        "fieldName": "",
                        "identifier": "LABEL",
                        "identifierName": "Label"
                    },
                    "operator": "IN",
                    "values": []
                }

```

Now:

```json
 "idFilter": {
                    "field": {
                        "fieldId": "labels.key",
                        "fieldName": "",
                        "identifier": "LABEL_V2",
                        "identifierName": "Label V2"
                    },
                    "operator": "IN",
                    "values": []
                }
```
{% endtab %}

{% tab title="Other Cloud Providers" %}
Earlier every request had the Label field as:

```json
                {
                    field": {
                        "fieldId": "labels.value",
                        "fieldName": "key",
                        "identifier": "LABEL",
                        "identifierName": "Label"
                    },
                    "operator": "IN",
                    "values": [
                        "value1"
                    ]
                } 
```

Now the request has the Label V2 field as:

```json

                {
                    field": {
                        "fieldId": "labels.value",
                        "fieldName": "key",
                        "identifier": "LABEL_V2",
                        "identifierName": "Label V2"
                    },
                    "operator": "IN",
                    "values": [
                        "value1"
                    ]
                }
```

Similarly, for labels.key:

Earlier:

```json
 "idFilter": {
                    "field": {
                        "fieldId": "labels.key",
                        "fieldName": "",
                        "identifier": "LABEL",
                        "identifierName": "Label"
                    },
                    "operator": "IN",
                    "values": []
                }

```

Now:

```json
 "idFilter": {
                    "field": {
                        "fieldId": "labels.key",
                        "fieldName": "",
                        "identifier": "LABEL_V2",
                        "identifierName": "Label V2"
                    },
                    "operator": "IN",
                    "values": []
                }
```
{% endtab %}
{% endtabs %}

Refer to the following API docs for details:

* [Create a Perspective](https://apidocs.harness.io/perspectives#operation/createPerspective)
* [Update a Perspective](https://apidocs.harness.io/perspectives#operation/updatePerspective)
{% endtab %}
{% endtabs %}

## Organize Perspectives using Folders <a href="#organize-perspectives-using-folders" id="organize-perspectives-using-folders"></a>

You can organize Perspectives by adding them to folders. The number of Folders that **can be created is 2000.**

Click **New folder**, name the folder, and then select the Perspectives you want to add.

<figure><img src="../../.gitbook/assets/folder-perspectives.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

You can also add a Perspective to a folder when you create it or move it to a folder when you edit it.

<figure><img src="../../.gitbook/assets/folder-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/move-folder.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% @harness-feedback/feedback %}
