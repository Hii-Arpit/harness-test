---
description: >-
  Review, filter, resolve, and ignore the cost anomalies that Harness detects
  for your Kubernetes clusters, cloud accounts, and AI providers.
---

# Anomalies

Infrastructure changes, new services, or inefficient resource utilization can lead to unexpected fluctuations in cloud expenses. Harness Cloud & AI Costs anomaly detection identifies unusually high cost spikes across your Kubernetes clusters, cloud accounts, and AI providers, and promptly notifies users, ensuring better cost control and transparency.

Detection runs on its own once cost data flows in. This page covers what you do with the anomalies it finds. Go to [How anomaly detection works](../../concepts/how-anomaly-detection-works.md) to understand how the Prophet model sets the expected spend that each anomaly is measured against.

![The Anomalies page, with summary cards above the Active tab](../../.gitbook/assets/anomalies-list.png)

## What you will learn from this topic

- How to read the summary cards and the Active, Resolved, and Archived/ignored tabs.
- How to find an anomaly with views, filters, search, and the date range.
- What each column, severity level, and confidence score means.
- How to open an anomaly and find the resources that caused it.
- How to resolve or ignore an anomaly, and how to export anomalies to CSV.

## Before you begin

- **A connected provider:** Connect your cloud provider accounts (AWS, GCP, Azure), Kubernetes clusters, or AI providers to Harness Cloud & AI Costs. Go to [Choose a provider](../../connect-your-data/choose-a-provider.md) to set up a connector.
- **Cost history:** Once connected, Harness automatically begins collecting cost data from your environments. The system requires approximately 42 days of historical data for optimal anomaly detection.
- **No setup for detection:** After data starts flowing in, the anomaly detection system automatically begins analyzing your cost patterns and identifying potential anomalies. No additional configuration is required for basic anomaly detection.
- **Permissions:** You need the **Anomalies: View** permission, which lets you view anomalies irrespective of cost view permissions. Go to [Permissions and RBAC](../../concepts/permissions-and-rbac.md) to review the Cloud & AI Costs roles.

{% hint style="info" %}
**Contact Harness Support**

If Anomalies does not appear in the Cloud & AI Costs navigation, contact your account administrator or [Harness Support](mailto:support@harness.io).
{% endhint %}

## Review the queue

To open the page, go to **Cloud & AI Costs** and select **Anomalies**. The page provides a comprehensive view of all detected anomalies across your cloud infrastructure and AI providers.

### Summary cards

Four cards at the top of the page summarize the anomalies in the selected date range. The cards stay the same when you switch between the Active, Resolved, and Archived/ignored tabs.

| Card | Description |
|---|---|
| **Active anomalies** | The total count of currently active anomalies requiring your attention. |
| **Unusual spend** | The total additional cost incurred due to active anomalies (difference between actual and expected spend). |
| **Unusual spend from anomalies (YTD)** | The total additional cost incurred from anomalies since the start of the year. This card does not follow the date range. |
| **Mean resolution time** | The average time taken to resolve anomalies from detection to resolution. The card shows `-` until an anomaly is resolved. |

### Active, Resolved, and Archived/ignored

Below the cards, the anomalies are organized into three tabs. Each tab shows its count.

- **Active:** Newly detected anomalies that require attention and have not been addressed yet.
- **Resolved:** Anomalies that have been reviewed and marked as resolved by your team. Resolved anomalies remain in this tab for reference and are automatically archived after 90 days.
- **Archived/ignored:** A long-term repository for anomalies that do not require active attention. An anomaly appears here when it meets one of the following conditions:
  - It was marked as **Ignore anomaly**.
  - It was recomputed and found to not qualify as an anomaly.
  - It is older than 90 days and was auto-archived.

Cloud providers (AWS, GCP, Azure) frequently update their billing data days or even weeks after initial reporting. Harness keeps anomaly data accurate and up to date with the latest billing information:

- **Automatic cost recalculation:** Anomaly costs are automatically recalculated daily based on the latest billing data. Active anomalies from the last 30 days are reprocessed. Anomalies that no longer meet threshold criteria are automatically archived.
- **Multi cost type support:** All cost types are recalculated and stored, respecting your preferred cost type settings. For AWS, these are unblended, blended, amortized, net amortized, and effective cost. For Azure, these are actual and amortized cost.
- **Dynamic criticality updates:** Anomaly severity is automatically recalculated when billing data changes.

## Find an anomaly

### Views

Two tabs below the page title let you organize and view anomalies in two different ways:

- **By resources:** Displays anomalies grouped by individual cloud resources. This view helps you identify which specific resources are experiencing unusual cost patterns.
- **By cost buckets:** Displays anomalies organized by the cost buckets in your cost categories. This view is useful for understanding cost anomalies at a higher organizational level rather than at the individual resource level. The table does not show the **Account** and **Product/service** columns, and it does not name the cost bucket. Open the anomaly to see its **Cost category** and **Cost bucket**. Go to [Cost categories and shared costs](../../concepts/cost-categories-and-shared-costs.md) to learn how cost buckets are defined.

### Filters, search, and date range

- **Search:** Enter text in **Search** to find anomalies in the current tab.
- **Filters:** Click **Add filter** to filter anomalies by multiple dimensions:
  - **Generic filters:** Total actual spend, Unusual spend, Cost view, Cost category, Payer account
  - **Cluster filters** (for Kubernetes): Cluster name, Namespace, Workload name, Service name
  - **GCP filters:** GCP project, GCP product, GCP SKU description
  - **AWS filters:** AWS account, AWS service, AWS usage type
  - **Azure filters:** Azure subscription, Azure meter categories, Azure resource group
  - **OpenAI filters:** OpenAI project, OpenAI model, OpenAI usage type
  - **Anthropic filters:** Anthropic project, Anthropic model, Anthropic usage type
- **Date range:** Click **Date** to change the time range. Quick select options under **Relative dates** include **Last 7 days**, **Last 30 days**, and **Last 90 days**, or select **Select custom range** to pick specific start and end dates. The default is **Last 30 days**.
- **Reset:** Click **Reset** to clear the filters you applied.
- **Save:** Click **Save** to store the filters that are currently applied.
  1. Enter a **Filter ID**. The ID can contain letters, numbers, `_`, and `$`.
  2. Under **Visibility**, select **Everyone** or **Only Me**.
  3. Click **Save**.
- **Saved filters:** The filter is listed under **Saved filters**. Select it to apply it. While it is applied, the control shows the filter ID. To delete a saved filter, open **Saved filters**, click the delete icon on that filter, and click **Delete**. Deleting a filter is permanent and cannot be undone.

Harness provides filtering support for anomalies based on cloud account identifiers, ensuring alignment with cost view-based access control (RBAC) settings.

| Cloud provider | Supported filter | Limitations |
|---|---|---|
| **AWS** | AWS account ID | Nested cost categories not supported |
| **GCP** | GCP project ID | Nested cost categories not supported |
| **Azure** | Azure subscription ID | Nested cost categories not supported |

## Read a row

### Table columns

The Active tab lists all active anomalies, sorted by **Unusual spend** with the highest first. A provider icon next to the date shows where the anomaly occurred.

| Column | Description |
|---|---|
| **Date** | The date when the unusual spend was observed. |
| **Duration** | How long the anomalous spending pattern persisted, in days since the anomaly started. |
| **Severity** | The severity level (Low, Medium, Critical) based on cost impact and percentage increase. |
| **Account** | The cloud account or cluster where the anomaly occurred: the AWS account, GCP project, or Azure subscription. For cluster-level anomalies, shows the cluster name. |
| **Product/service** | The specific cloud service and product that caused the anomaly. For GCP, the product and SKU. For AWS, the service and usage type. For Azure, the service and meter category. |
| **Actual spend** | The actual amount spent over anomaly duration. |
| **Expected spend** | The predicted cost over anomaly duration, based on historical patterns. |
| **Unusual spend** | The difference between actual and expected spend over anomaly duration. This is the cost impact of the anomaly. |
| **% increase** | The percentage increase over expected spend: `((Actual spend - Expected spend) / Expected spend) × 100` |
| **Alerts** | The alert recipients for the anomaly. Hover over the cell to see the email and Slack alert recipients. |

The Resolved and Archived/ignored tabs use their own columns:

| Column | Tab | Description |
|---|---|---|
| **Resolved by/on** | Resolved | Shows who resolved the anomaly and when it was resolved. |
| **Time taken** | Resolved | The duration from anomaly detection to resolution. |
| **Added on** | Archived/ignored | When the anomaly was added to the Archived/ignored tab (either when it was ignored or when it was archived). |
| **Detected** | Archived/ignored | The date when the anomaly was originally detected. |
| **Status** | Archived/ignored | Indicates whether the anomaly was manually ignored or automatically archived. |

Both tabs also show **Severity**, **Account**, **Product/service**, **Actual spend**, and **Unusual spend**, and the Resolved tab shows **Date**.

To show or hide columns, click **Columns** above the table and select the columns you want. **Date**, **Duration**, **Severity**, **Total actual spend**, and **Alerts** are always shown. Select **Reset** to restore the default columns.

### Severity

The **Severity** column and the details page show **Critical**, **Medium**, or **Low**. Severity is based on the cost impact and the percentage increase. **Critical** is the highest impact. **Low** is a minor deviation.

Severity is recalculated when billing data changes, so an anomaly can move to a lower level after a recalculation.

### Confidence

When an anomaly is detected, the system assigns a confidence score (0 to 100%) representing how confident the system is that the cost change is a true anomaly versus normal variance. The score is computed by analyzing how far the actual cost deviates from the expected range, based on historical spending patterns. Higher scores indicate a spike unlikely to be normal variation.

| Score | Confidence | What it tells you |
|---|---|---|
| 1 to 25% | Low | Small deviation, could be normal day-to-day variance |
| 25 to 50% | Moderate | Noticeable deviation, likely a real anomaly |
| 50 to 75% | High | Clear deviation from historical pattern |
| 75 to 100% | Very high | Unmistakable anomaly, far beyond expected cost range |

The confidence score appears on the anomaly details page, for example **60% - High**.

## Open an anomaly

Select any row in the table to open the details page for that anomaly. For each anomaly detected, Harness provides insights into which resources might be causing the anomaly.

![The details page for an anomaly, with the spend summary, cost trend, and top resource cost changes](../../.gitbook/assets/anomalies-details.png)

The top of the page shows the **Severity**, **Confidence**, **Date**, and **Duration** of the anomaly. Depending on the anomaly, it also shows details such as the account, payer account, and service for AWS, the cluster for Kubernetes, or the **Cost category** and **Cost bucket** for a cost bucket anomaly. An Archived anomaly shows an **Archived anomaly** badge, and a resolved one shows **Marked as resolved**.

### Spend summary

- **Expected spend:** The predicted cost based on historical patterns.
- **Actual spend:** The actual amount spent during the anomaly period.
- **Total unusual spend:** The difference between actual and expected spend, shown as both dollar amount and percentage increase (for example, +$329.72 (+197%)).

The page states which AWS cost type the spend values use, for example unblended. Go to [Anomaly settings](alerts-and-the-ignore-list.md) to change how AWS, Azure, and GCP costs are shown.

### Cost trend

**Cost trend for anomaly duration** is a graph that displays historical data for 60 or 90 days. Select **60d** or **90d** to change the range. The graph shows:

- **Spend within expected range**
- **Anomaly**, the anomalous spend that exceeded thresholds

Click **Open in Cost Views** to explore the same cost data in a cost view. Go to [Cost views](../../cost-visibility/cost-views/README.md) to learn how to filter and group it.

For an anomaly that lasts more than one day, **Spend breakdown by days for multi-day anomaly** lists the **Actual spend (Expected)** and **Unusual spend** for each day.

### Top resource cost changes

**Top resource cost changes** is a detailed breakdown of the resources that contributed most to the anomaly. For each **Resource**, the table shows its **Tags** and its **One-day change**, which compares the day before the anomaly to when it started. Click **Download as .csv** to export the table.

To build this list, Harness queries the cloud provider's cost data and identifies resources that have experienced significant cost increases compared to previous periods. The system:

- Aggregates total costs for each resource
- Computes cost increases or decreases compared to the previous day
- Ranks resources by highest cost increase
- Stores anomalies in a time series database with metadata (reported cost, expected cost, anomaly score)
- Flags anomalies for investigation and corrective action

## Resolve or ignore an anomaly

Move an anomaly between tabs when you have reviewed it. From the table, click the **More** icon at the end of the row. From the details page of an active anomaly, click **Actions**. Then select an option:

| Tab | Options |
|---|---|
| **Active** | **Mark as resolved:** Move the anomaly to the Resolved tab. **Ignore anomaly:** Move the anomaly to the Archived/ignored tab. |
| **Resolved** | **Mark as active:** Move the anomaly back to the Active tab if it needs further investigation. **Ignore anomaly:** Move the anomaly to the Archived/ignored tab. |
| **Archived/ignored** | No actions are available. |

Ignoring an anomaly affects only that anomaly. To exclude an expected cost pattern from future detection, create an ignore list rule. Go to [Anomaly settings](alerts-and-the-ignore-list.md) to set up ignore list rules.

## Export to CSV

Export filtered anomalies to CSV for further analysis, reporting, or integration with other tools.

1. Click **Export CSV**.
2. Under **Export scope**, select one of the following:
   - **All filtered results:** Export all anomalies matching your current filters.
   - **Current page only:** Export only the anomalies visible on the current page.
3. Review **Filters applied**, which lists the filters the export uses, such as the **Date** range and the anomaly **Status**.
4. Click **Download**.

![The Export anomalies as .csv dialog](../../.gitbook/assets/anomalies-export-csv.png)

## Troubleshooting

<details>

<summary>I received an anomaly alert for a particular day, but not on that day, it came later in the month. Why?</summary>

This can happen because anomaly alerts are triggered when the anomaly is detected, not when it occurred. There is no delay between detection and alerting. However, if the cost data available at the time did not indicate an anomaly, the system would not have flagged it initially. Later in the month, once cost data was refreshed and a spike became apparent, the anomaly was detected. To account for such cases, Harness reruns the anomaly detection job for the past 3 days, which is likely why the alert was sent at a later time.

</details>

<details>

<summary>An anomaly moved from Active to Archived/ignored without anyone ignoring it</summary>

Anomaly costs are recalculated daily against the latest billing data. If the cost falls within the expected range after a recalculation, the anomaly is auto-archived, and its details page shows an **Auto-archived** note.

</details>

<details>

<summary>Does anomaly detection just look at cost, or can it also look at usage?</summary>

Currently, anomaly detection only monitors cost.

</details>

## Next steps

Once you have reviewed your anomalies, decide who is notified and which anomalies are surfaced.

{% content-ref url="alerts-and-the-ignore-list.md" %}
[Anomaly settings](alerts-and-the-ignore-list.md)
{% endcontent-ref %}

- [How anomaly detection works](../../concepts/how-anomaly-detection-works.md): Understand how expected spend and anomalies are calculated.
- [Budgets](../budgets/README.md): Set spend limits and get alerted before you exceed them.

{% @harness-feedback/feedback %}
