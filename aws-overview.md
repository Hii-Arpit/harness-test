---
description: "Explore the AWS Cost Dashboard in CACM to track total spend, forecasted costs, top accounts, and trending services across your AWS environment"
hidden: true
---


# AWS

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22Kubernetes%22%2C%22slug%22%3A%22kubernetes%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fkubernetes-overview%22%7D%2C%7B%22label%22%3A%22AWS%22%2C%22slug%22%3A%22aws%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Faws-overview%22%7D%2C%7B%22label%22%3A%22GCP%22%2C%22slug%22%3A%22gcp%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fgcp-overview%22%7D%2C%7B%22label%22%3A%22Azure%22%2C%22slug%22%3A%22azure%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fazure-overview%22%7D%5D" selectedPlatform="aws" %}

{% tabs %}
{% tab title="AWS Cost Dashboard" %}
### AWS Cost dashboard

The AWS dashboard provides comprehensive cost visibility across your AWS environment. It displays your total AWS costs with trends, forecasted costs based on historical data, top 20 AWS accounts by spend, trending services showing cost increases or decreases, historical and forecasted cost charts, current vs previous period comparisons, and the top 5 most expensive services by month. This dashboard helps you track, analyze, and forecast your AWS cloud spending effectively.

<figure><img src="../../../.gitbook/assets/dashboard-overview.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

**Dimensions**

Dimensions are key metrics and data visualizations that provide specific insights into your AWS cloud costs. Each dimension in the AWS Cost Dashboard focuses on a particular aspect of your cloud spending, such as total costs, forecasts, account-level spending, or service-specific trends.

| **Dimensions**                         | **Description**                                                                                                                                                                                   |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Total Cost                             | The total AWS cost with cost trend.                                                                                                                                                               |
| Forecasted Cost                        | The forecasted cloud cost with cost trend. Forecasted cost is the prediction based on your historical cost data, and it is predicted for the same future time period as your selected time range. |
| Top 20 AWS accounts                    | The cost of the top 20 AWS account you are using to connect Harness to AWS via a Harness AWS Cloud Provider.                                                                                      |
| Top Trending Services                  | The top AWS services by cost increase or decrease                                                                                                                                                 |
| Historical and Forecasted Cost         | The historical and forecasted AWS cost. Forecasted cost is the prediction based on your historical cost data and it is predicted for the same future time period as your selected time range.     |
| Current Period vs Last Period          | The cost of the current and previous time range.                                                                                                                                                  |
| Top 5 Most Expensive Services by Month | Top five services that incurred the maximum cost per month.                                                                                                                                       |

**Interacting with the AWS Cost Dashboard**

**Basic Controls**

* **Time Filter**: Select **Time Range** to filter data using pre-defined options: Last 7 days, Last 30 days, Last 90 days, Last 12 months, Last 24 months. After selecting, click the **Refresh** icon to update the data
* **Dashboard Options**:
  * **Clear Cache and Refresh**: Updates the dashboard with the latest data
  * **Download**: Export the dashboard as PDF or CSV with options to:
    * Set custom page size
    * Expand tables to show all rows
    * Arrange dashboard tiles in a single column
  * **Filter Icon**: Toggle filter visibility

**Exploring Dashboard Elements**

* **Cost by AWS Account**:
  * Use up/down arrows to navigate through the list
  * View percentage contribution of each AWS account to total cost
* **Historical and Forecasted Cost**:
  * Click on the chart to drill down by time period
  * Toggle between **Visualization** (graph) and **Table** views
  * Download specific data to your local system
* **Most Expensive Services by Month**:
  * Click on chart elements to explore detailed cost breakdowns
  * Further drill down by time period in the resulting view
  * View filtered cost data with precise metrics

**Additional Actions**

* **Download Dashboard**: Export the entire dashboard for offline analysis or sharing
  * For more information, go to [Download Dashboard Data](https://developer.harness.io/harness-ai/use-harness-platform/harness-dashboards/dashboard-legacy/download-dashboard-data).
{% endtab %}

{% tab title="Orphaned EBS Volumes &amp; Snapshots" %}
### View orphaned EBS volumes and snapshots dashboard

<figure><img src="../../../.gitbook/assets/ebs-volumes.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Perform the following steps to view Orphaned EBS Volumes and Snapshots Dashboard:

1. In Harness, click **Dashboards**.
2. Click **Orphaned EBS Volumes and Snapshots Dashboard**.
3. In **EBS Volume Creation Date**, select the date. You can add multiple OR conditions.
4.  In **EBS Volume Cost Date**, select the date range.

    By default, **This Month** is selected.

* **Presets**: Select a Preset filter. For example, Today, Yesterday, etc.
* **Custom**: Custom allows you to select the date range.

5. In **Snapshot Creation Date**, select the date. You can add multiple OR conditions.
6. Once you have selected all the filters, click **Update**.

The **Orphaned EBS Volumes and Snapshots Dashboard** is displayed.
{% endtab %}

{% tab title="EC2 Instance Metrics" %}
<figure><img src="../../../.gitbook/assets/instance-metrics.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Perform the following steps to view AWS EC2 Instance Metrics Dashboard:

1. In Harness, click **Dashboards**.
2. Click **AWS EC2 Instance Metrics Dashboard**.
3. Select **EC2 Instance Id** from the drop-down list for which you want to view the details. You can select multiple IDs.
4. In **Metrics start time date**, select the time duration. You can select the preset value or use custom.\
   By default, the **Last 30 Days** is selected.
   1. **Presets**: Select a Preset filter. For example, Today, Yesterday, etc.
   2. **Custom**: Custom allows you to select the date range.
5. Select the Public IP Address.
6. Once you have selected all the filters, click **Update**.

The **EC2 Instance Metrics Dashboard** is displayed.
{% endtab %}

{% tab title="EC2 Inventory Cost" %}
### AWS EC2 inventory Cost dashboard

<figure><img src="../../../.gitbook/assets/invetory-cost.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Perform the following steps to view AWS Cost Dashboard:

1. In Harness, click **Dashboards**.
2. Click **AWS EC2 Inventory Cost Dashboard**.
3. Select the Current State.
4. Select the **Region**.
5. Select the **AWS Account**.
6.  Select **EC2 Last updated time Date**.

    By default, the **Last 30 Days** is selected.

    1. **Presets**: Select a Preset filter. For example, Today, Yesterday, etc.
    2. **Custom**: Custom allows you to select the date range.
7. Drag the slider to define the **Current CPU Max (%)**.
8. Select the value for **Public IP Address**.
9. Once you have selected all the filters, click **Update**.

The **AWS EC2 Inventory Cost Dashboard** is displayed.
{% endtab %}

{% tab title="Resource Breakdown" %}
### View AWS resource breakdown dashboard

<figure><img src="../../../.gitbook/assets/resource-breakdown.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Perform the following steps to view AWS Resource Breakdown Dashboard:

1. In Harness, click**Dashboards**.
2. Select**By Harness**and click**AWS** **Resource Breakdown**.

The **AWS Resource Breakdown Dashboard** is displayed:

| **Dimension**                 | **Description**                                     |
| ----------------------------- | --------------------------------------------------- |
| Monthly Cost Breakdown        | Includes the monthly cost of all the AWS resources. |
| Resource Level Cost Breakdown | Includes the resource level cost breakdown.         |

3. In **Reporting Timeframe**, select the time duration.
4. In **Account**, select the account for which you want to view the cost. You can select multiple accounts.
5. In **Service**, select the service for which you want to view the cost. You can select multiple Services.
6. In **Region**, select the region. You can select multiple regions.
7. Once you have made all the selections, click **Update**. The data is refreshed with the latest data from the database.
{% endtab %}
{% endtabs %}

<details>

<summary>SCAD-Related Columns for AWS (Click to expand)</summary>

[Split cost allocation data (SCAD)](https://docs.aws.amazon.com/cur/latest/userguide/split-cost-allocation-data.html) feature introduces cost and usage data for container-level resources-specifically ECS tasks and Kubernetes pods-into AWS Cost and Usage Reports (CUR). Previously AWS CUR was not including granular level K8S and ECS cost visibility. Now, split cost allocation calculates container-level costs by analyzing each container's consumption of EC2 instance resources, assigning costs based on the amortized cost of the instance and the percentage of CPU and memory resources utilized by containers running on it.

CACM has support for analyzing K8S cost data via SCAD CUR. Following are the columns which can be used.

* `parentresourceid`
* `reservedusage`
* `actualusage`
* `splitusage`
* `splitusageratio`
* `splitcost`
* `netsplitcost`
* `unusedcost`
* `netunusedcost`
* `publicondemandsplitcost`
* `publicondemandunusedcost`

Along with the following labels:

* `aws:eks:cluster-name`
* `aws:eks:deployment`
* `aws:eks:namespace`
* `aws:eks:node`
* `aws:eks:workload-name`
* `aws:eks:workload-type`

</details>

{% @harness-feedback/feedback %}
