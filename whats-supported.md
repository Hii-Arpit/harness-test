---
description: Supported platforms and feature support matrix for Harness CACM.
title: What's supported in Harness CACM
---


# What's supported in Harness CACM

{% include "../.gitbook/includes/__shared/shared/ccm-supported-platforms.md" %}

***

### Supported AI Providers and Capabilities <a href="#supported-ai-providers-and-capabilities" id="supported-ai-providers-and-capabilities"></a>

The following table displays which Cloud & AI Cost Management capabilities are available for each AI provider. Core capabilities (GenAI costs, AI traces, Cost Explorer, Dashboards, Cost Categories, and Budgets) are supported across every provider. **AWS Bedrock** has the widest coverage, supporting every capability.

| Capability                                                             | AWS Bedrock | Google Vertex | Azure Foundry | Anthropic Enterprise | Anthropic Developer Platform | OpenAI | Cursor | Devin | GitHub Copilot |
| ---------------------------------------------------------------------- | :---------: | :-----------: | :-----------: | :------------------: | :--------------------------: | :----: | :----: | :---: | :------------: |
| GenAI costs (provider, model, sub-account ID, token type, token count) |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| Costs by principal                                                     |      ✅      |       ❌       |       ❌       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| AI traces (agent, service, custom dimensions)                          |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| Cost Explorer / Views                                                  |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| Dashboards                                                             |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| Cost Categories                                                        |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| Budgets                                                                |      ✅      |       ✅       |       ✅       |           ✅          |               ✅              |    ✅   |    ✅   |   ✅   |        ✅       |
| User-level AI budgets and enforcement                                  |      ✅      |       ❌       |       ❌       |           ✅          |               ❌              |    ❌   |    ✅   |   ✅   |        ✅       |
| Anomaly detection                                                      |      ✅      |       ✅       |       ✅       |           ❌          |               ✅              |    ✅   |    ❌   |   ❌   |        ❌       |

{% hint style="info" %}
For **Anthropic Developer Platform** and **OpenAI**, costs by principal are attributed by API key.
{% endhint %}

***

### Supported Environments <a href="#supported-environments" id="supported-environments"></a>

Harness CACM supports the following platforms and orchestration systems:

#### Cloud Platforms <a href="#cloud-platforms" id="cloud-platforms"></a>

* AWS
* GCP
* Azure

#### Container Orchestration <a href="#container-orchestration" id="container-orchestration"></a>

* Kubernetes: EKS (AWS), GKE (GCP), AKS (Azure)
* ECS Clusters

#### Deployment Model <a href="#deployment-model" id="deployment-model"></a>

* Harness SaaS

{% hint style="info" %}
Go to [Data Sources and Refresh Rates](data-ingestion-reference.md) to review what CACM ingests from each provider, the source, and how often it refreshes.
{% endhint %}

***

#### Supported Kubernetes Management Platform <a href="#supported-kubernetes-management-platform" id="supported-kubernetes-management-platform"></a>

The following section lists the support for Kubernetes management platform for CACM:

| **Technology**               | **Supported Platform** | **Pricing**      |
| ---------------------------- | ---------------------- | ---------------- |
| OpenShift 3.11               | GCP                    | GCP              |
| OpenShift 4.3                | AWSOn-Prem             | AWSCustom-rate\* |
| Rancher                      | AWS                    | Custom-rate\*\*  |
| Kops (Kubernetes Operations) | AWS                    | AWS              |

* Cost data is supported for On-Prem OpenShift 4.3. This uses a custom rate.
* Cost data is supported for K8s workloads on AWS managed by Rancher, but the cost falls back to the custom rate.

***

## CACM Feature Flags <a href="#cacm-feature-flags" id="cacm-feature-flags"></a>

Some Harness CACM features are released behind feature flags to get feedback from specific customers before releasing the features to the general audience.

{% hint style="info" %}
To enable a feature flag in your Harness account, contact [Harness Support](mailto:support@harness.io).
{% endhint %}

| **Flag**                                      | **Description**                                                                                             |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `CCM_CLUSTER_ORCH`                            | Enables cluster orchestrator functionality                                                                  |
| `CCM_COMMORCH`                                | Enables the commitment orchestrator in the UI side nav                                                      |
| `CCM_COMMORCH_RDS`                            | Enables RDS support in commitment orchestration                                                             |
| `CCM_COMMORCH_ELASTICACHE`                    | Enables ElastiCache support in commitment orchestration                                                     |
| `CCM_CURRENCY_PREFERENCES`                    | Enables viewing costs in preferred currency                                                                 |
| `CCM_BUDGET_CASCADES`                         | Enables nested budgets for Financial Management                                                             |
| `CCM_COST_CATEGORIES_DASHBOARD`               | Enables the use of cost categories in the dashboard                                                         |
| `CCM_ENABLE_DATA_SCOPE`                       | Enables RBAC on CACM data scope                                                                             |
| `CCM_GOVERNANCE_EVALUATION_COST_PER_RESOURCE` | Enables cost per resource for a governance evaluation                                                       |
| `CCM_ANOMALIES_V2`                            | Enables the new version of CACM anomalies                                                                   |
| `CCM_COST_CATEGORY_STAMPED_DATA`              | Enables the use of stored cost categories in Perspectives                                                   |
| `CCM_ANOMALIES_CONDITIONAL_DRILL_DOWN`        | Shows perspective link when drill down is missing for anomalies                                             |
| `CCM_DISABLE_STATISTICAL_ANOMALIES`           | Disables statistical model anomalies and shows only prophet-based anomalies                                 |
| `CCM_ANOMALIES_PROPHET_V2`                    | Enables Prophet V2 model for anomaly detection                                                              |
| `CCM_ANOMALY_LOOKBACK_RECOMPUTATION`          | Revises existing anomalies with latest cost data                                                            |
| `CCM_ANOMALY_EXTENDED_RERUN_DAYS`             | Reruns anomaly detection on past 15 days instead of 4 days                                                  |
| `CCM_ANOMALY_REQUIRE_BOTH_THRESHOLDS`         | Requires both cost and percentage thresholds for anomaly filtering                                          |
| `CCM_ANOMALY_WHITELISTING`                    | Enables ignore list rule in anomalies                                                                       |
| `CCM_ANOMALY_EXCLUDE_RECURRING_FEES`          | Excludes recurring fees, commitments, taxes, credits, and adjustments from anomaly detection                |
| `CCM_ANOMALY_CC_OTHER_FILTER_BYPASS`          | Bypasses additional non-cost-category filters for cost category anomalies in perspectives                   |
| `CCM_ANOMALY_EXCLUDE_MARKETPLACE`             | Excludes marketplace charges from anomaly detection                                                         |
| `CCM_ANOMALIES_COST_TYPES_CALCULATION`        | Enables anomaly lookback recalculation with multi-cost-type support                                         |
| `CCM_RECOMMENDATION_AWS_PASSTHROUGH_V2`       | Uses AWS Cost Optimization Hub instead of AWS Cost Explorer for recommendations                             |
| `CCM_RECOMMENDATION_AWS_PASSTHROUGH_V2_READ`  | Enables API filtering for AWS passthrough recommendations V2                                                |
| `CCM_RECOMMENDATION_COST_TYPES`               | Enables cost type support for recommendations                                                               |
| `CCM_NODE_POOL_RECOMMENDATIONS_V2`            | Enables node pool recommendations V2                                                                        |
| `CCM_FILTER_AUTOSCALED_NODEPOOLS`             | Filters out autoscaled node pools from recommendations                                                      |
| `CCM_WORKLOAD_RECOMMENDATIONS_V2`             | Enables workload recommendations V2                                                                         |
| `CCM_INVENTORY_V2`                            | Enables cloud asset inventory for AWS resources in Asset Governance                                         |
| `CCM_GCP_INVENTORY_V2`                        | Enables cloud asset inventory for GCP resources in Asset Governance                                         |
| `CCM_AZURE_INVENTORY_V2`                      | Enables cloud asset inventory for Azure resources in Asset Governance                                       |
| `CCM_EXTERNAL_DATA_INGESTION`                 | Enables ingestion of external cost data from third-party vendors via FOCUS CSV format                       |
| `CCM_UNIT_COST_METRICS`                       | Enables unit cost metrics for tracking custom business metrics alongside cloud costs                        |
| `CCM_PERSPECTIVES_V2`                         | Enables the new Cost Explorer interface (users can switch between Cost Explorer and classic Perspectives)   |
| `CCM_MSP`                                     | Enables margin obfuscation for managed service providers                                                    |
| `CCM_ENABLE_OIDC_AUTH_AWS`                    | Enables OIDC authentication for AWS connectors                                                              |
| `CCM_ENABLE_OIDC_AUTH_GCP`                    | Enables OIDC authentication for GCP connectors                                                              |
| `CCM_AWS_NEW_CUR`                             | Enables CUR 2.0 (Data Exports) support for AWS billing                                                      |
| `CCM_TRIGGER_AZURE_COST_EXPORT`               | Enables on-demand triggering of Azure Cost Management exports for fresher billing data (Enterprise only)     |
| `PL_ENABLE_USER_IMPERSONATION`                | Enables user impersonation for the platform                                                                  |

{% @harness-feedback/feedback %}
