---
description: >-
  A comprehensive guide to Harness Cloud & AI Cost Management (CACM)
  subscription plans, features, and licensing options.
---


# Harness CACM: Subscription Plans

Harness offers two primary licensing tiers for Cloud & AI Cost Management: **Free Forever** and **Enterprise**.

{% hint style="info" %}
* **If Your Cloud Spend Exceeds $250K/year**: You must upgrade to an Enterprise plan. The Free Forever plan is limited to organizations with less than USD250K/month in cloud spend.
* **If Your Cloud Spend is Under $250K/year**: CACM remains accessible on the Free Forever plan, but with certain limitations and adjustments.
{% endhint %}

## Free forever <a href="#free-forever" id="free-forever"></a>

The Free Forever plan is a no-commitment, no-contract plan that provides you with the essential features of CACM for free. It is ideal for small and medium-sized organizations with less than USD250K/month in cloud spend.

| Feature                                                                                 | Free Forever                                  |
| --------------------------------------------------------------------------------------- | --------------------------------------------- |
| **Cloud Spend Manage**                                                                  | Up to $250,000/year                           |
| **Kubernetes Clusters**                                                                 | 2                                             |
| [**AutoStopping Rules**](https://developer.harness.io/cloud-cost-management/cost-optimization/autostopping-rules/autostopping-rules) | 10 rules                                      |
| **Data Visibility**                                                                     | Data available for viewing limited to 30 days |
| **Currency Preferences**                                                                | Not supported                                 |

**Definitions**


* **Cloud Spend Manage**: The total monthly cloud expenditure across all connected cloud accounts (AWS, Azure, GCP).
* **Kubernetes Clusters**: Number of Kubernetes clusters you can connect for cost visibility and optimization.
* **AutoStopping Rules**: Intelligent rules that automatically stop idle cloud resources to reduce costs.
* **Data Visibility**: The timeframe for which historical cost data and analytics are accessible to users in the dashboard and reports.
* **Currency Preferences**: Ability to view and report costs in currencies other than USD.

## Enterprise Plan <a href="#enterprise-plan" id="enterprise-plan"></a>

Harness has introduced a new pricing model for Cloud & AI Cost Management that gives you the flexibility to choose exactly what you need. Our CACM suite is now available as four separate SKUs that can be purchased individually or bundled together.

You can buy SKUs individually or buy the entire CCM bundle.

The individual SKUs available are:

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><strong>Cloud Cost Insights</strong><br>Gain complete visibility into your cloud spend across AWS, Azure, GCP, and Kubernetes.<br><br><strong>Includes:</strong><ul><li>Perspectives</li><li>Cost Categories</li><li>BI Dashboards</li><li>Recommendations</li><li>Asset Governance</li><li>Budgets</li><li>Anomalies</li></ul></td><td></td></tr><tr><td><strong>Commitment Orchestrator</strong><br>Optimize your cloud commitments with intelligent management across your cloud providers.<br><br><strong>Includes:</strong><ul><li>Reserved Instance Recommendations</li><li>Savings Plans Optimization</li><li>Commitment Analysis</li><li>Coverage Planning</li></ul></td><td></td></tr></tbody></table>

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><strong>AutoStopping</strong><br>Automatically stop idle non-production resources to reduce cloud costs by up to 75%.<br><br><strong>Includes:</strong><ul><li>Idle Resource Detection</li><li>Smart Scheduling</li></ul></td><td></td></tr><tr><td><strong>Cluster Orchestrator</strong><br>Optimize Kubernetes infrastructure costs with advanced resource management.<br><br><strong>Includes:</strong><ul><li>Advanced Bin-Packing</li><li>Spot Instance Management</li><li>Intelligent Workload Scheduling</li><li>Multi-Level Spot/On-Demand Distribution</li><li>Karpenter-Based Autoscaling</li></ul></td><td></td></tr></tbody></table>

## Subscription management <a href="#subscription-management" id="subscription-management"></a>

<figure><img src="../.gitbook/assets/subscription-management.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Navigate to the **Account Settings** -> **Subscriptions**. Your subscription details will show:

* **Subscription Details**
  * **Account Name**: Your Harness account identifier
  * **Plan**: Your current subscription tier (Free, Team, or Enterprise)
  * **Subscription Type**: Shows which CACM SKUs you have purchased. The tooltip will show the number of SKUs purchased alongwith which ones.
  * **Subscribed Limits**: Maximum usage thresholds for your plan per each SKU purchased
  * **Start Date**: When your current subscription period began
  * **Expiry Date**: When your subscription will need renewal
* **Usage vs Subscribed Limit:** For each purchased SKU, you will see your current usage metrics compared to your subscription limits on an yearly basis.

{% hint style="info" %}
**TOTAL CLOUD SPEND MANAGED**

Usage for CACM SKUs is measured against your **Total Cloud Spend Managed** - the cloud spend Harness CACM actively manages. It is calculated by subtracting EDP (Enterprise Discount Program) discounts and marketplace spend from your total unblended cloud spend.

```text
Total Cloud Spend Managed = Total Unblended Spend - EDP Discounts - Marketplace Spend
```
{% endhint %}

{% hint style="info" %}
Features that are not included in your current subscription will be clearly marked with a lock icon (🔒) in the UI. You can click on these locked features to see details about their benefits and how to add them to your subscription.

<img src="../.gitbook/assets/locked-screen.png" alt="Click to view full size image" data-size="original">
{% endhint %}

## Free Forever to Enterprise upgrade <a href="#upgrading-from-free-forever-to-enterprise" id="upgrading-from-free-forever-to-enterprise"></a>

When you upgrade from Free Forever to Enterprise plan, the following changes occur:

| Capability                                              | After upgrade to Enterprise License                                                        | Business Advantage                                                                                                                  |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| **All reporting, optimization and governance features** | As per purchased SKUs                                                                      | Reduce cloud spend through advanced optimization recommendations, custom dashboards, and predictive analytics                       |
| **Kubernetes Clusters**                                 | Unlimited                                                                                  | Achieve complete visibility across your entire K8s infrastructure, enabling organization-wide cost allocation and optimization      |
| **AutoStopping Rules**                                  | Unlimited                                                                                  | Maximize cloud cost savings by automatically stopping idle resources across all environments and projects                           |
| **Data Visibility**                                     | Extended to 5 years. Users will gain all the historical access of their free tier as well. | Make data-driven decisions with long-term trend analysis and gain deeper insights into seasonal patterns and multi-year cost trends |

## Enterprise license expiry <a href="#enterprise-license-expiry" id="enterprise-license-expiry"></a>

When your Enterprise license expires, the following changes occur:

| Feature                                                                           | Behavior After License Expiry                                              | Impact                                                                      |
| --------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Data Collection**                                                               | Stops for all sources                                                      | No new cost data is collected from any cloud provider or Kubernetes cluster |
| **Historical Data**                                                               | Remains accessible                                                         | Previously collected data can still be viewed but no new data is added      |
| [**Recommendations**](https://developer.harness.io/docs/category/recommendations) | No longer generated                                                        | Existing recommendations remain visible but no new ones are created         |
| **Alerts & Notifications**                                                        | Stopped                                                                    | No budget alerts, anomaly alerts, or scheduled reports are sent             |
| [**AutoStopping**](https://developer.harness.io/cloud-cost-management/cost-optimization/autostopping-rules/autostopping-rules) | AutoStopping rules will not show new savings due to paused data collection | No new Savings calculation data is displayed                                |

Users will see **license expired** banners in the UI to inform them of the expired status.

## Need help? <a href="#need-help" id="need-help"></a>

If you have questions about your subscription options or need assistance with upgrading:

* **Contact Sales**: [Harness Sales Team](https://www.harness.io/company/contact-sales?utm_source=harness_io&utm_medium=cta&utm_campaign=platform&utm_content=pricing&utm_term=essentials)
* **Support Portal**: [Harness Support](https://support.harness.io)
* **Schedule a Demo**: [Request a personalized demo](https://www.harness.io/demo)

{% @harness-feedback/feedback %}
