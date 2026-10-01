---
description: >-
  What Cloud & AI Cost Management ingests from each provider, where the data
  comes from, and how often it refreshes.
tags:
  - cloud-cost-management
---


# Data Sources and Refresh Rates

Cloud & AI Cost Management (CACM) ingests cost, pricing, inventory, usage, and recommendation data from each connected provider. This page lists what CACM reads, the source it reads from, and how often each data type refreshes, for AWS, GCP, Azure, and Kubernetes or ECS clusters.

### What you will learn from this topic <a href="#what-you-will-learn-from-this-topic" id="what-you-will-learn-from-this-topic"></a>

* The data types CACM ingests from each provider.
* The source system each data type comes from.
* The default update frequency for each data type.

***

### AWS <a href="#aws" id="aws"></a>

CACM ingests AWS data from the Cost and Usage Report, the public pricing API, and account inventory.

| Data type                          | Source                                                                                                                                               | Update frequency                                                      |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Billing / cost data                | AWS Cost and Usage Report (v1.0 or v2.0) or Billing Data Exports, delivered to a customer-owned S3 bucket and loaded into the CACM billing pipeline. | Daily by default, with hourly sync when configured.                   |
| Price lists / unit rates           | AWS public pricing API and the CUR itself, with a fallback to standardized values when public pricing is not available.                              | On demand during cost computation, cached for the calculation window. |
| Resource inventory & configuration | AWS account tags, IAM cross-account roles, and inventory snapshots.                                                                                  | Tags collected daily; inventory discovered every two hours.           |
| Service usage / consumption        | CUR line items, including usage amount, usage type, operation, and resource tags.                                                                    | Matches the report cadence: daily or hourly.                          |
| Recommendations / right-sizing     | AWS Compute Optimizer and Trusted Advisor.                                                                                                           | Daily at midnight UTC.                                                |

***

### GCP <a href="#gcp" id="gcp"></a>

CACM ingests GCP data from the Cloud Billing export to BigQuery, public pricing datasets, and Asset Inventory.

| Data type                          | Source                                                                                               | Update frequency                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Billing / cost data                | GCP Cloud Billing export to BigQuery or GCS, which Harness queries directly.                         | Daily by default, with an optional hourly job every eight hours. |
| Price lists / unit rates           | BigQuery public pricing datasets and the billing export.                                             | On demand during cost computation.                               |
| Resource inventory & configuration | GCP Asset Inventory exported to GCS.                                                                 | Loaded whenever new inventory paths appear.                      |
| Service usage / consumption        | BigQuery billing rows, including usage amount, usage unit, cost, credits, labels, project, and zone. | Matches the export cadence.                                      |
| Recommendations / right-sizing     | GCP Recommender API.                                                                                 | Daily.                                                           |

***

### Azure <a href="#azure" id="azure"></a>

CACM ingests Azure data from the Cost Management billing export, the Retail Prices API, and subscription inventory.

| Data type                          | Source                                                                                    | Update frequency                                                       |
| ---------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Billing / cost data                | Azure Cost Management billing export to an Azure Storage container.                       | Hourly by default, triggered every hour.                               |
| Price lists / unit rates           | Azure Retail RateCard / Retail Prices API.                                                | On demand for cost and recommendation calculations.                    |
| Resource inventory & configuration | Subscription and resource-group inventory, plus subscription-to-name mappings.            | Subscription mapping daily; inventory refreshes as new exports arrive. |
| Service usage / consumption        | Azure billing export, including quantity, meter category, resource group, tags, and cost. | Hourly or daily, depending on export configuration.                    |
| Recommendations / right-sizing     | Azure Advisor and VM right-sizing recommendations.                                        | Daily.                                                                 |

***

### Kubernetes and ECS clusters <a href="#kubernetes-and-ecs-clusters" id="kubernetes-and-ecs-clusters"></a>

CACM derives cluster costs from events and utilization metrics reported by the Harness in-cluster agent, combined with public cloud pricing for the underlying nodes.

| Data type                          | Source                                                                                                                                                         | Update frequency                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Billing / cost data                | Kubernetes and ECS events and utilization metrics reported by the Harness in-cluster agent.                                                                    | Events and utilization fetched every two minutes and processed hourly; final billing (SCAD plus instance) rolled up daily. |
| Price lists / unit rates           | Public cloud pricing for the underlying node families (AWS, GCP, and Azure on-demand and spot), plus any customer-defined custom rates.                        | On demand during cost allocation.                                                                                          |
| Resource inventory & configuration | Kubernetes API and metrics-server, through the delegate's perpetual task, covering nodes, pods, namespaces, workloads, services, labels, limits, and requests. | Hourly, through the Kubernetes event and utilization sync.                                                                 |
| Service usage / consumption        | Real CPU and memory utilization, request and limit data, pod counts, and idle capacity.                                                                        | Aggregated hourly, with a daily cost roll-up.                                                                              |
| Recommendations / right-sizing     | Kubernetes workload and node right-sizing recommendations, plus ECS task and node recommendations.                                                             | Daily.                                                                                                                     |

***

### Related concepts <a href="#related-concepts" id="related-concepts"></a>

* Go to [AWS integration](../integrations/cloud-providers/aws.md) to connect an AWS account.
* Go to [GCP integration](../integrations/cloud-providers/gcp.md) to connect a GCP account.
* Go to [Azure integration](../integrations/cloud-providers/azure.md) to connect an Azure account.
* Go to [Kubernetes integration](../integrations/kubernetes.md) to connect a cluster.
* Go to [What is supported in Harness CACM](whats-supported.md) to review supported platforms and capabilities.

{% @harness-feedback/feedback %}
