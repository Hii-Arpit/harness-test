---
description: >-
  Learn about the recommendations summary page and the various actions that you
  can perform on this page.
---


# Recommendations in Harness CACM

{% hint style="info" %}
After you enable CACM, it may take up to 48 hours for the recommendations to appear in Cloud Costs. It depends on the time at which CACM receives the utilization data for the service.
{% endhint %}

{% hint style="success" %}
🚀 **What's New?**

We have recently introduced AWS Passthrough Recommendations as a major enhancement to Harness Cloud & AI Cost Management that provides **intelligent, action-oriented recommendations** for your AWS resources. Instead of just suggesting instance type changes, the system now analyzes your resources and recommends specific actions to optimize costs and performance. **Please note, we have moved from Cost Explorer Hub to Cost Optimization Hub as a part of this change.**
{% endhint %}

## What are Recommendations? <a href="#what-are-recommendations" id="what-are-recommendations"></a>

Harness CACM Recommendations are data-driven insights that help you optimize your cloud resources for cost efficiency without compromising performance. By analyzing historical usage patterns, resource configurations, and cloud provider pricing models, CACM identifies opportunities to right-size resources, eliminate waste, and implement best practices.

**Benefits of Using CACM Recommendations**

* **Reduce Azure VM Costs**: Identify underutilized virtual machines and receive rightsizing or termination suggestions to significantly reduce Azure spending.
* **Optimize AWS EC2 Instances**: Detect idle or oversized EC2 instances and get actionable guidance to select optimal instance types or eliminate unnecessary resources.
* **Clean Up Container Registry**: Identify and remove unused container images in Amazon ECR, reducing storage costs and improving registry management.
* **Enhance Cost Governance**: Implement best practices for tagging, business unit allocation, and spending patterns across AWS, GCP, and Azure to maintain financial control.
* **Improve Kubernetes Node Efficiency**: Balance cost and performance with optimized node pool configurations for your Kubernetes clusters, tailored to each cloud provider's specific requirements.
* **Right-Size Kubernetes Workloads**: Analyze CPU and memory utilization patterns to properly configure resource requests and limits, preventing waste while ensuring application performance.

## Recommendations Homepage <a href="#recommendations-homepage" id="recommendations-homepage"></a>

<figure><img src="../../../.gitbook/assets/recommendations-output.gif" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Recommendation Type**: CACM has six types of recommendations: `AZURE_INSTANCE`, `EC2_INSTANCE`, `GOVERNANCE`, `NODE_POOL` and `WORKLOAD`. See details about all the types of recommendations [here](./#recommendations-per-cloud-provider).
* **Cloud Provider**:
* **+ Add Filter**
  * **Generic Filters**: Cloud Account ID, Cloud Account Name, Resource ID, Resource Name, Region, Cost Category, Cluster Labels, Cloud Tags, Potential Spend(USD), Savings (USD), Governance Rule Name
  * **AWS-Specific Filters**: Instance Type
  * **Azure-Specific Filters**: VM Size, Resource Group
  * **Container-Specific Filters**: Kubernetes Cluster Name, Kubernetes Namespace, ECS Cluster Name, ECS Launch Type

<figure><img src="../../../.gitbook/assets/outputwo.gif" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Export CSV:** Option to export your Recommendations as comma-separated values (CSV) files. Exporting allows you to use the data in other software. Export respects the filters applied by the user in the filter panel. Only comma-separated values files (CSV) are supported and the maximum number of rows allowed in one export is 10,000 rows.
* **Ignore & Reject Lists:** This contains: Ignore list and Rejected Auto-Inferences.
  * **Ignore List:** Adding resources to the Ignore list will stop Harness from displaying recommendations for those resources. You can view the Ignore list with details by clicking on "Manage Ignore List" on the overview page.
  * **Rejected Auto-Inferences:** Recommendations that were auto-inferred as applied but later rejected will appear here. Review rejection reasons and identify resources that may need to be ignored. You can see the rejected inference, recommendation type, rejected on, rejected by and on and the reason for rejection.

### Open and Applied Recommendations <a href="#open-and-applied-recommendations" id="open-and-applied-recommendations"></a>

{% tabs %}
{% tab title="Open Recommendations" %}
* **Open Recommendations**: List of [recommendations](./#recommendations-per-cloud-provider) that are not applied yet. [All the types of recommendations](./#recommendations-per-cloud-provider) are listed here. You can view:
  * Potential Monthly Savings that can be achieved with the recommendation
  * Potential Monthly Spend without applying recommendations.
  * Table with columns:
    * Resource Name
    * Recommended Action/Resource
    * Potential Monthly Savings
    * Potential Monthly Spend
    * Jira/Servicenow Ticket Status
{% endtab %}

{% tab title="Applied Recommendations" %}
*   **Applied Recommendations:** When you click on an individual recommendation, you'll be able to view a detailed breakdown of the recommendation, including relevant insights, suggested actions, and any supporting information. See how to apply recommendations [here](./#apply-recommendations).

    <figure><img src="../../../.gitbook/assets/output-hist.gif" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

The Applied Recommendations tab provides comprehensive visibility into your cost optimization efforts with the following key components:

* **Savings Realized:** Displays the total financial impact of all implemented recommendations, helping you quantify the ROI of your cost optimization efforts.
* **Recommendations vs. Savings Chart:** A visual trend analysis showing the correlation between applied recommendations and actual cost savings over time. The saving from applied recommendations is shown against the number of recommendations applied.
* **Breakdown of Marked as Applied:** Shows the distribution of applied recommendations by number of recommendations and associated savings to identify which optimization strategies are most frequently implemented.
* **Applied Recommendations:** Number of recommendations marked as applied and the timeperiod.
* **Details Table:** Provides a comprehensive view of each applied recommendation with the following information:
  * Resource name
  * Recommended Action/Resource
  * Monthly Savings
  * Associated ticket
  * Applied on
  * Applied by (Also shows if a recommendation was inferred )

{% hint style="info" %}
Marking a recommendation as "Applied" assumes all resources are actioned and full savings are recorded. If only a subset is applied, the user must manually update the realized savings using the “Edit savings amount” option.
{% endhint %}
{% endtab %}
{% endtabs %}

### Recommendation Settings <a href="#recommendation-settings" id="recommendation-settings"></a>

{% tabs %}
{% tab title="Preferences" %}
<figure><img src="../../../.gitbook/assets/recommendations-preferences.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **General Preferences:**
  * **Automatically detect when recommendations are applied** - Enables auto-detection of applied recommendations in your infrastructure.
  * **Show Recommendations on Parent Resources** - Display recommendations on Nodepool/EC2/ECS Services.
  * **Show Recommendations on Child Resources** - Display recommendations on workloads.
  * **Show Recommendations on Resources added to the IgnoreList** - Display recommendations for resources in the ignore list.
* **Resource Specific Preferences for Harness Generated Recommendations:** Recommendation Preferences for Harness Generated Recommendations (node pool and workload)

<figure><img src="../../../.gitbook/assets/account-specific.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Account-specific Resource Preferences for Cloud Provider Recommendations**: Account-Specific Resource Preferences allow you to customize recommendation calculation parameters for individual AWS cloud accounts. Instead of using a single set of recommendation parameters across all accounts, you can define different tuning parameters (presets) for specific accounts based on their unique requirements. For each resource type (Workload, Node Pool, ECS Service, EC2 Instance):
  1. Click **View Details** to open the account overrides drawer
  2. You'll see:
     * **Default Preset**: The preset applied to all accounts by default
     * **Account-Specific Presets**: Custom presets assigned to specific accounts

  **Adding an Account Override**
  1. Click **View Details** for a resource type
  2. In the "Account-Specific Presets" section, click **Add Account-Specific Preset**
  3. In the modal:
     - **Select Accounts**: Choose one or more AWS accounts
     - **Select Preset**: Choose the preset to apply to these accounts
     - Optionally click **Create Preset** to create a new preset
  4. Click **Apply** to add the override
  5. Click **Save Changes** to persist your changes

<figure><img src="../../../.gitbook/assets/set-preset.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

{% hint style="info" %}
New Recommendation Preferences may take up to 24 hours to fully update across the platform because preferences are applied during the next scheduled batch processing job. However, changes will be reflected immediately on the drill-down page, while the Overview page may take additional time to reflect updates.
{% endhint %}
{% endtab %}

{% tab title="Manage Presets" %}
<figure><img src="../../../.gitbook/assets/manage-presets.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

This helps users to create and save customized configurations for their recommendations. These presets capture specific user preferences, such as tuning parameters for resource types like workloads, nodepools, ECS, and EC2 instances.  

Users can fine-tune recommendations for different resource types by configuring specific tuning parameters and save presets. By default, Harness CACM has default presets for all resources but users can tune recommendations using custom values. To set custom values, click on the recommendation and expand the "Tune Recommendations" section to configure the tuning parameters. 

{% tabs %}
{% tab title="Workload" %}
<figure><img src="../../../.gitbook/assets/workload-preset.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

| Parameter | Description |
| --- | --- |
| **Quality of Service (QoS)** | Choose between Burstable or Guaranteed resource allocation for workloads. Burstable allows resources to exceed requests up to limits, while Guaranteed sets requests equal to limits for stable performance. |
| **Percentage Buffer for CPU/Memory** | Additional resource margin to handle unexpected spikes in usage. Higher buffer values provide more headroom for workload fluctuations but increase resource allocation. |
{% endtab %}
{% tab title="Nodepool" %}
<figure><img src="../../../.gitbook/assets/nodepool-preset.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

| Parameter | Description |
| --- | --- |
| **Minimum Node Count** | Ensures high availability by maintaining a minimum number of nodes in the cluster. This prevents scaling down below a threshold that might impact application availability. |
| **Percentage Buffer for CPU/Memory** | Additional resource margin to handle unexpected spikes in usage. Helps prevent resource contention during peak loads while maintaining efficient resource utilization. |
{% endtab %}
{% tab title="AWS EC2" %}
<figure><img src="../../../.gitbook/assets/ec-preset.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

| Parameter | Description |
| --- | --- |
| **Instance Family Selection** | <ul><li><strong>Within the Same Instance Family:</strong> Recommendations will suggest optimized instance types within the same instance family, ensuring workload compatibility and minimizing migration complexity.</li><li><strong>Across Instance Families:</strong> Recommendations can suggest optimized instance types across different instance families, potentially unlocking greater cost savings and efficiency improvements.</li></ul> |
{% endtab %}
{% tab title="AWS ECS" %}
<figure><img src="../../../.gitbook/assets/ecs-preset.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

| Parameter | Description |
| --- | --- |
| **Buffer Percentage** | Additional resource margin to ensure containers have sufficient resources during peak usage. Helps prevent throttling and performance issues while maintaining efficient resource allocation. |
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Jira Settings" %}
<figure><img src="../../../.gitbook/assets/ticketing-tool-mapping.png" alt=""><figcaption>Click to view full size image</figcaption></figure>

**1. Applied/Ignored Status Mapping**

Recommendations supports Jira Status Mapping. This feature allows you to automatically align recommendation states with the statuses of your Jira issues.

In **Recommendation Settings**, you can define which Jira statuses correspond to recommendations being considered **Applied** or moved to the **Ignore List**. When a linked Jira issue reaches a mapped status, the recommendation is automatically updated. You can also choose **Resolution Codes** with which the recommendation is automatically updated to **Applied**.

- **Jira Statuses that move recommendation to Applied automatically**: Choose from a list of Jira statuses. Within an hour, CCM checks if any open recommendation is linked to a Jira issue with a mapped status, the recommendation is automatically updated to **Applied**.

- **Jira Statuses that move recommendation to Ignore List automatically**: Choose from a list of Jira statuses. Within an hour, CCM checks if any open recommendation is linked to a Jira issue with a mapped status, the recommendation is automatically updated to **Ignore List**. You can also choose **Resolution Codes** with which the recommendation is automatically updated to **Ignore List**.

{% hint style="info" %}

- A Jira connector must be successfully configured for using the feature. 
- This is not supported for ServiceNow.
- Statuses not specified will follow the default recommendation flow.
- Regardless of automatic status updates, users can still manually move recommendations to either the Applied or Ignore List at any time.
- Changes will apply to all future status updates.
{% endhint %}

**2. Actual Savings Field Mapping**
Map Jira custom fields to capture actual savings from applied recommendation tickets. When tickets reach the statuses configured in Section 1, the savings values from your Jira custom fields are synced to CACM. For each mapping, you need to specify:

- **Jira Project**: Select one or more Jira projects where this mapping applies
- **Issue Type**: Select one or more issue types (e.g., "Story", "Task", "Bug")
- **Savings Field**: Select the numeric custom field that contains actual savings values


**3. Default Jira Projects for Cost Categories**

Set default Jira projects for cost recommendations. When creating tickets, the project is auto-selected based on the resource's first matching cost category below. You may change the project during ticket creation.

By mapping cost categories to specific Jira projects, you ensure that recommendations reach the right stakeholders without manual routing, reducing response time and increasing the likelihood of implementation. This is especially valuable in large organizations where different teams are responsible for different resource types. 

To configure this feature, click on **+Add mapping** to add a new mapping, then select the cost category, cost bucket, and Jira project.

 <figure><img src="../../../.gitbook/assets/cc.png" alt=""><figcaption>Click to view full size image</figcaption></figure>
{% endtab %}

{% tab title="Cost Settings" %}
Cost settings control which cost basis is used to calculate the **Potential Monthly Spend** and **Potential Monthly Savings** displayed across all recommendations. These settings inherit from your Account Settings defaults. Override them here to apply only to Recommendations.

To configure cost settings, navigate to **Recommendations** → **Settings** → **Cost Settings**.

<figure><img src="../../../.gitbook/assets/recommendations-cost-settings.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
Cost type changes take effect on the next job run, which may take up to 24 hours.
{% endhint %}

{% tabs %}
{% tab title="Amazon Web Services" %}
CACM surfaces AWS recommendation costs for three resource types.

**Passthrough recommendation costs (EC2)**

EC2 recommendations are passthrough. CACM pulls them directly from [AWS Cost Optimization Hub](https://docs.aws.amazon.com/cost-management/latest/userguide/coh-preferences.html#coh-savings-estimation) without recalculating costs. The cost type shown in Harness reflects the savings estimation setting configured in your AWS Console, not a setting you change in Harness.

{% hint style="info" %}
Changes made in the AWS Console are reflected in Harness CACM after the next scheduled sync.
{% endhint %}

| Cost type | Maps to | Description |
|---|---|---|
| Before discounts | Unblended | On-demand cost before Reserved Instance or Savings Plan discounts are applied. |
| After discounts | Net-amortized | Cost after Reserved Instance and Savings Plan discounts are applied. |

**Nodepool recommendation costs** and **Workload recommendation costs**

Both settings share the same four cost type options:

| Cost type | Description |
|---|---|
| List price | Base on-demand price before any discounts or reservation savings are applied. |
| Unblended | Standard pay-as-you-go billing cost without reservation discounts. |
| Amortized | Reservation and Savings Plan costs spread evenly over the reservation term. |
| Net-amortized | Amortized cost further reduced by any negotiated discounts and credits. |

The default cost type for AWS workload costs is **Amortized**.
{% endtab %}

{% tab title="Google Cloud Provider" %}
**Nodepool recommendation costs**

| Cost type | Description |
|---|---|
| List price | Base on-demand price with no discounts or credits applied. No additional toggles. |
| Actual | Effective billed cost. When selected, you can include or exclude specific credits and discounts using the toggles below. |

When **Actual** is selected, the following toggles are available:

Savings Programs:

| Toggle | Description |
|---|---|
| Spend-based CUD discounts | Discounts from Committed Use contracts based on a committed spend amount. |
| Legacy spend-based CUD credits | Legacy form of spend-based committed use credits. |
| Resource-based CUD credits | Discounts from Committed Use contracts tied to specific vCPU and memory resources. |

Invoice Level Charges:

| Toggle | Description |
|---|---|
| Tax | Applicable taxes included in the cost calculation. |

Other Savings:

| Toggle | Description |
|---|---|
| Promotional credits | One-time credits from Google promotions or trials. |
| Sustained use discounts (SUDs) | Automatic discounts for running Compute Engine resources for a significant portion of the billing month. |
| Spending-based discounts | Discounts earned by maintaining a minimum monthly spend commitment. |
| Subscription credits | Credits from GCP subscription agreements. |
| Negotiated savings | Custom pricing negotiated with Google Cloud. |

**Workload recommendation costs**

| Cost type | Description |
|---|---|
| List price | Base on-demand price with no discounts or credits applied. |
| Actual | Effective billed cost after applying the credits and discounts configured for Nodepool above. |

The default cost type for GCP workload costs is **Actual**.
{% endtab %}

{% tab title="Microsoft Azure" %}
VM and VMSS recommendations are passthrough. CACM pulls them directly from Azure Advisor without recalculating costs. The cost type you select here controls which Azure billing view those passthrough costs are matched against. Nodepool and Workload costs are calculated by CACM using the selected cost type.

All three resource types (**Passthrough (VM, VMSS)**, **Nodepool**, and **Workload**) share the same three cost type options:

| Cost type | Description |
|---|---|
| List price | Retail price before any enterprise agreements, negotiated discounts, or credits are applied. |
| Actual | The billed cost as it appears on your invoice. For reservations, the full charge appears in the billing period of purchase. |
| Amortized | Reservation costs spread evenly across the reservation term. Use this for apples-to-apples comparisons between committed and on-demand spend. |

The default cost type for Azure workload costs is **Actual**.
{% endtab %}
{% endtabs %}

{% endtab %}
{% endtabs %}

## Recommendations per Cloud Provider <a href="#recommendations-per-cloud-provider" id="recommendations-per-cloud-provider"></a>

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22Kubernetes%22%2C%22slug%22%3A%22kubernetes%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Frecommendations%2F1-home-recommendations%2Fkubernetes-1-home-recommendations%22%7D%2C%7B%22label%22%3A%22AWS%22%2C%22slug%22%3A%22aws%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Frecommendations%2F1-home-recommendations%2Faws-1-home-recommendations%22%7D%2C%7B%22label%22%3A%22GCP%22%2C%22slug%22%3A%22gcp%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Frecommendations%2F1-home-recommendations%2Fgcp-1-home-recommendations%22%7D%2C%7B%22label%22%3A%22Azure%22%2C%22slug%22%3A%22azure%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Frecommendations%2F1-home-recommendations%2Fazure-1-home-recommendations%22%7D%5D" %}

## Kubernetes Workload Utilization Aggregations <a href="#kubernetes-workload-utilization-aggregations" id="kubernetes-workload-utilization-aggregations"></a>

For a Kubernetes workload recommendation, **Aggregation** refers to how CPU and memory utilization values are combined across your workload. Utilization can be aggregated using one of the following methods:

* **Time-weighted:** Weights CPU and memory utilization by the active duration of each pod.
* **Absolute:** Sums CPU and memory utilization values without adjusting for pod duration.

### Example 1 <a href="#example-1-example1" id="example-1-example1"></a>

Let's assume you want to check the CPU requests of your workload between 3 A.M. and 4 A.M. Imagine there were two pods during that duration:

* Each pod requesting 0.4 CPU
* The first pod was deleted at 3:53 A.M. The first pod was active for 53 minutes.
* The second pod was created at 3:53 A.M. The second pod was active for 7 minutes.

For **time-weighted**, the utilization value is calculated as:

`[(cpu request of pod 1) * (active time) + (cpu request of pod 2) * (active time)] / total duration = [(0.4*53) + (0.4*7)]/60 = 0.4`

For **absolute**, the utilization value is calculated as:

`(cpu request of pod 1) + (cpu request of pod 2) = 0.4 + 0.4 = 0.8`

### Example 2 <a href="#example-2-example2" id="example-2-example2"></a>

Let's assume you want to check the CPU requests of a workload with three pods in your cluster:

* Each pod requesting 0.4 CPU
* Pod 1 runs from 0-25 mins into the hour
* Pod 2 runs from 15-40 mins into the hour
* Pod 3 runs from 35-60 mins into the hour

For **time-weighted**, the utilization value is calculated as:

`[(cpu request of pod 1) * (active time) + (cpu request of pod 2) * (active time) + (cpu request of pod 3) * (active time)] / total duration = ((0.4*25) + (0.4*25) + (0.4*25))/60 = 0.5`

For **absolute**, the utilization value is calculated as:

`(cpu request of pod 1) + (cpu request of pod 2) + (cpu request of pod 3) = 0.4 + 0.4 + 0.4 = 1.2`

***

## Apply Recommendations <a href="#apply-recommendations" id="apply-recommendations"></a>

Applying recommendations is easy! You just need to:

1. **Review Recommendations** - Analyze the suggested optimizations in the Recommendations dashboard.
2. **Tune Parameters** - Adjust recommendation settings to see how different configurations affect potential cost savings. See [Recommendations per Cloud Provider](./#recommendations-per-cloud-provider) for a deeper drilldown into a particular type of recommendation to understand the action required, cost calculations and tuning.
3. **Implement Changes and Track using Jira/ServiceNow** - Apply the optimizations manually in your cloud environment. [Create and manage Jira or ServiceNow tickets](./#managing-recommendations-via-jira-servicenow-tickets) to monitor implementation progress.
4. **Update Status** - Mark recommendations as applied in the CACM platform once implemented. Once applied, recommendations show up in the **Applied** tab.

### Auto Inferences <a href="#auto-inferences" id="auto-inferences"></a>

Auto Inferences is an intelligent feature in Harness Cloud & AI Cost Management (CCM) that automatically detects when recommendations have been implemented and tracks the actual savings realized. This eliminates the need for manual tracking and provides accurate ROI measurement for your cost optimization efforts.

Auto Inferences runs as part of daily batch jobs. Typically, implemented recommendations are detected within 24-48 hours after the recommendation expires.

**What is Auto Inferences?**

When you receive recommendations (such as rightsizing EC2 instances, changing Azure VM SKUs, or implementing governance policies), Auto Inferences automatically monitors your infrastructure to detect if these recommendations have been applied in your cloud environment. Once detected, the system:

* Marks the recommendation as "Applied"
* Calculates and records the actual monthly savings achieved

When a recommendation is auto-inferred, it shows up with a banner.

<figure><img src="../../../.gitbook/assets/auto-inference-banner.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

You can choose to verify the recommendation or reject it. Upon clicking verify, you can choose to confirm whether the inferred savings matched the actual amount saved to ensure more accurate savings calculations.

<figure><img src="../../../.gitbook/assets/verify-inference.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

If you reject an inference, it will show under **Rejected Auto-Inferences** in **Ignore and Reject Lists**

<figure><img src="../../../.gitbook/assets/rejected-inference.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

**How It Works**

Auto Inferences runs as a scheduled background process that:

* **Identifies Candidate Recommendations:** Scans for recommendations that have passed their validity period and focuses on recommendations that haven't been manually marked as applied or ignored
* **Monitors Infrastructure State:** Continuously queries your cloud infrastructure inventory and compares current resource configurations against recommended changes
* **Validates Implementation:** Checks if resources match the recommended configuration and verifies that changes align with the original recommendation
* **Records Results:** Automatically marks validated recommendations as "Inferred as Applied" and tracks actual monthly savings realized from the implementation

**Enabling Auto Inferences**

Auto Inferences is controlled at the account level through Recommendation Preferences:

1. Navigate to **Cloud & AI Cost Management** → **Recommendations** → **Settings** → **Preferences**
2. Locate the **Automatically detect when recommendations are applied** toggle under General Preferences
3. Save your preferences

We also have added new filters to the recommendations page to help you manage auto-inferred recommendations under "Verification Status":

* **Inferred - Pending Review**: Recommendations automatically detected as implemented but awaiting your verification. You can verify, reject, or edit the savings amount.
* **Inferred - Verified**: Auto-detected recommendations that have been verified by a user and are confirmed as accurately applied.

{% hint style="info" %}
**IMPORTANT**

* Currently, Auto Inferences supports AWS EC2 instances and Azure Virtual Machines recommendations. Support for additional cloud providers and resource types is planned for future releases.
* Manual actions take precedence. Once you manually mark a recommendation as applied or ignored, Auto Inferences will not override your action.
{% endhint %}

***

## Managing Recommendations via Jira/ServiceNow Tickets <a href="#managing-recommendations-via-jiraservicenow-tickets" id="managing-recommendations-via-jiraservicenow-tickets"></a>

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

1. Create a Jira or ServiceNow connector:
   * Steps to create a Jira connector: [Create Jira Connector](https://developer.harness.io/harness-ai/use-harness-platform/connectors/ticketing-systems/connect-to-jira)
   * Steps to create a ServiceNow connector: [Create ServiceNow Connector](https://developer.harness.io/harness-ai/use-harness-platform/connectors/ticketing-systems/connect-to-service-now)
2. Configure ticketing tool settings:
   * Navigate to **Cloud Costs** > **Setup** > **Default Settings** > **Cloud & AI Cost Management**.
   * Under **Ticketing preferences**, select the **Ticketing tool** and the **Ticketing tool connector**. The default ticketing tool is **Jira**. You can choose **ServiceNow** if that's the tool used in your organization. Switching your ticketing tool between Jira and ServiceNow results in the removal of the existing recommendation tickets.

<figure><img src="../../../.gitbook/assets/ticketing-tool-selector.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Go to the **Recommendations** page and create tickets to apply recommendations.

1. Select **Create a ticket**. In case you haven't set up your ticketing tool settings on the account level, you will see a prompt guiding you to access the **Default Settings** page to configure both the ticketing tool and the associated connector.
2. Enter the following ticket details:

{% tabs %}
{% tab title="Jira" %}
* **Jira project** — Select the Jira project where you want to create a ticket. Go to [Create Jira Issues in CD Stages](https://developer.harness.io/continuous-delivery/use-continuous-delivery/cd-building-blocks/cd-steps/ticketing-systems/create-jira-issues-in-cd-stages).
* **Issue type** — Select a Jira issue type from the list of types in the Jira project you selected. Go to [Create Jira Issues in CD Stages](https://developer.harness.io/continuous-delivery/use-continuous-delivery/cd-building-blocks/cd-steps/ticketing-systems/create-jira-issues-in-cd-stages).
* **Ticket summary** — Add a summary of the issue.
* **Description** — Add a description for the issue.
{% endtab %}

{% tab title="ServiceNow" %}
* **Ticket Type** - Select the ticket type from the dropdown list. For example, change request, Data Management task, and so on. Based on the selected ticket type, you might need to enter more required inputs.
* **Short Description** - Enter a brief description of the task. This is the title of the ticket.
* **Description** - Enter a more detailed description about the recommendation.

<figure><img src="../../../.gitbook/assets/servicenow_Example.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

The Description field contains relevant information about the recommendation for which this ticket was created. Harness CACM retrieves the following data from ServiceNow:

When a user opens the dialog box to create a ServiceNow ticket, a request is made to obtain all possible ticket types. Here is a sample response:

```json
{
    "status": "SUCCESS",
    "data": [
        {
            "key": "asset_reclamation_request",
            "name": "Asset Reclamation Request"
        },
        {
            "key": "asset_task",
            "name": "Asset Task"
        },
        {
            "key": "business_app_request",
            "name": "Business Application Request"
        }
    ]
}
```

Once the user selects a ticket type, another request retrieves the fields associated with that ticket type to determine the required fields. Here is a sample response:

```json
{
    "data": [
        {
            "key": "parent",
            "name": "Parent",
            "required": false,
            "schema": {
                "array": false,
                "typeStr": "",
                "type": "unknown",
                "customType": null,
                "multilineText": false
            },
            "internalType": null,
            "allowedValues": [],
            "readOnly": false,
            "custom": false
        },
        {
            "key": "made_sla",
            "name": "Made SLA",
            "required": false,
            "schema": {
                "array": false,
                "typeStr": "boolean",
                "type": "boolean",
                "customType": null,
                "multilineText": false
            },
            "internalType": null,
            "allowedValues": [],
            "readOnly": false,
            "custom": false
        }
    ]
}
```

When the user clicks "Create Ticket," an API call is made to ServiceNow to create the ticket with the provided inputs. Additionally, there is an internal call that periodically checks if the ticket has been closed. Based on this status, the recommendation is moved to the applied state.
{% endtab %}

{% tab title="ServiceNow Service Request" %}
## ServiceNow Service Request Integration <a href="#servicenow-service-request-integration" id="servicenow-service-request-integration"></a>

In ServiceNow (SNOW), a Service Request is a type of ticket that users raise when they want a standard service, resource, or information from IT or another department.

**Common examples of Service Requests include:**

* Requesting access to software applications
* Ordering new hardware resources
* Creating new email distribution lists
* Requesting password resets
* Requesting system configurations or reports

### Steps to Configure a Catalog Item in ServiceNow <a href="#steps-to-configure-a-catalog-item-in-servicenow" id="steps-to-configure-a-catalog-item-in-servicenow"></a>

* Log in to ServiceNow as an Administrator.
* Navigate to the "Maintain Items" section.
* Click "New" in the top right corner to create a new catalog item.

<figure><img src="../../../.gitbook/assets/snow-five.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* Define a service catalog with the "Software" category. Ensure the name of the catalog item exactly matches "Harness CACM Recommendation" and mark it as Active.

<figure><img src="../../../.gitbook/assets/snow-six.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* Define variables in the catalog item so that recommendation payload can be sent with the request/ticket:

<figure><img src="../../../.gitbook/assets/snow-seven.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* Use camel case convention for variable names (e.g., awsAccountId). You can provide additional data fields such as description and shortDescription.

<figure><img src="../../../.gitbook/assets/snow-eight.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* The variables defined in ServiceNow will be automatically populated with values from the recommendation data (keys must match) in CCM UI.

<figure><img src="../../../.gitbook/assets/snow-ticket.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}
{% endtabs %}

## FAQs <a href="#faqs" id="faqs"></a>

<details>

<summary>What filtering options are available for recommendations across different cloud providers?</summary>

Harness provides filtering support for recommendations based on cloud account identifiers and Kubernetes attributes. This allows for better cost optimization insights while maintaining alignment with perspective-based RBAC settings.

* **AWS EC2**: Filtering is supported on AWS Account ID. Nested Cost Categories are not supported.
* **AWS ECS**: Filtering is supported on AWS Account ID. Nested Cost Categories are not supported.
* **Azure VM**: Filtering is not supported.
* **Kubernetes**: Filtering is supported on Labels and Cluster Name. Nested Cost Categories are not supported.
* **Governance Recommendations**:
  * **AWS**: No filtering support.
  * **Azure**: No filtering support.
  * **GCP**: No filtering support.

Filtering support for recommendations extends to **RBAC configurations based on perspective folder access settings**, ensuring that cost-saving suggestions are appropriately scoped to the right teams.

</details>

<details>

<summary>Why aren't memory metrics displayed when using Datadog integration?</summary>

If you ingest memory metrics using Datadog integration, EC2 recommendations do consider these metrics in their calculations; however, the memory utilization data is not displayed on the EC2 Recommendation page.

This occurs because:

* The CPU and memory metrics data we retrieve is typically sourced from CloudWatch
* In this case, the metrics originate from an external source (Datadog)
* These Datadog metrics are directly integrated with AWS Compute Optimizer and are utilized in generating the recommendations
* According to AWS Compute Optimizer API documentation, they do not offer support for retrieving these external utilization metrics

As a result, while the recommendations are accurately calculated using both CPU and memory data, the memory metrics themselves will not be visible in the recommendation interface.

Read more: [External metrics ingestion](https://docs.aws.amazon.com/compute-optimizer/latest/ug/external-metrics-ingestion.html)

</details>

<details>

<summary>Why should I evaluate recommendations before implementing them?</summary>

Before using recommendations in your environment, ensure that you evaluate their impact thoroughly. The person reviewing the recommendations should be able to understand the impacts identified in the recommendations, as well as the impact on the infrastructure and business.

Using recommendations without proper assessment could result in unexpected changes, such as:

* Performance degradation for critical workloads
* Reliability issues during peak usage periods
* Incompatibility with specific application requirements
* Business disruption if services become unavailable or slow

</details>

{% @harness-feedback/feedback %}
