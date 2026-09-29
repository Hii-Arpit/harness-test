---
title: ccm-supported-platforms
---

## SaaS <a href="#saas" id="saas"></a>

|                          | **Features**                                                                                                          | **Use Case**                                                                                                         | **AWS**                           | **Azure** | **GCP** | **Kubernetes** | **RBAC Support** |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | --------------------------------- | --------- | ------- | -------------- | ---------------- |
| **📊 Cost Reporting**    | [Perspectives](../../../../cost-reporting/perspectives/)                                                              | Custom views to slice and dice cloud spend across business dimensions.                                               | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **📊 Cost Reporting**    | [Cost categories](../../../../cost-reporting/cost-categories/)                                                        | Group and analyze cloud costs based on user-defined categories.                                                      | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **📊 Cost Reporting**    | [Dashboards](../../../../cost-reporting/bi-dashboards/)                                                               | Visualize and track cloud cost trends, anomalies, and budgets in one place.                                          | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **📊 Cost Reporting**    | [Anomalies](../../../../cost-governance/anomalies/)                                                                   | Automatically detect unusual spikes or drops in your cloud spend.                                                    | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **💸 Cost Optimization** | [AutoStopping](../../../../cost-optimization/autostopping-rules/)                                                     | Automatically shut down idle resources to save costs.                                                                | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **💸 Cost Optimization** | [Recommendations](../../../../cost-optimization/recommendations/)                                                     | Get actionable insights to right-size and optimize cloud resources.                                                  | ✅                                 | ✅         | ✅       | ✅              | ✅                |
| **💸 Cost Optimization** | [Cluster Orchestrator for AWS EKS clusters](../../../../cost-optimization/cluster-orchestrator-for-aws-eks-clusters/) | Automate provisioning, scaling, and shutdown of Kubernetes clusters based on workload patterns to reduce idle costs. | ✅ EKS                             |           |         |                | ✅                |
| **💸 Cost Optimization** | [Commitment Orchestrator](../../../../cost-optimization/commitment-orchestrator/)                                     | Manage and optimize AWS commitments like EC2 Convertible RIs and SPs and RDS Standard RIs .                          | <ul><li>EC2</li><li>RDS</li></ul> |           |         |                | ✅                |
| **🛡️ Cost Governance**  | [Asset Governance](../../../../cost-governance/asset-governance/)                                                     | Enforce policies on cloud resources to ensure cost efficiency and compliance.                                        | ✅                                 | ✅         | ✅       |                | ✅                |
| **🛡️ Cost Governance**  | [Budgets](../../../../cost-governance/budgets/)                                                                       | Set and track cloud spend limits to avoid budget overruns.                                                           | ✅                                 | ✅         | ✅       | ✅              | ✅                |

{% hint style="info" %}
Harness CACM does not currently support AWS China regions.
{% endhint %}

## Self-Managed Enterprise Edition <a href="#self-managed-enterprise-edition" id="self-managed-enterprise-edition"></a>

Review the following information about what installation infrastructure and CACM features are supported on Harness Self-Managed Enterprise Edition.

{% hint style="info" %}
AWS is the only supported installation infrastructure. If you do not install Harness Self-Managed Enterprise Edition on AWS, then you cannot use the CACM features.
{% endhint %}

### Connected Environment <a href="#connected-environment" id="connected-environment"></a>

| **Features**             | **AWS** | **Azure** | **GCP** | **Kubernetes** |
| ------------------------ | ------- | --------- | ------- | -------------- |
| Perspectives             | ✅       | ✅         | ✅       | ✅              |
| Cost categories          | ✅       | ✅         | ✅       | ✅              |
| Budgets                  | ✅       | ✅         | ✅       | ✅              |
| BI dashboards            | ✅       | ✅         | ✅       | ✅              |
| Anomaly detection        | ✅       | ✅         | ✅       | ✅              |
| Currency standardization | ❌       | ❌         | ❌       | ❌              |
| Recommendations          | ✅       | ✅         | ✅       | ✅              |
| AutoStopping             | ❌       | ❌         | ❌       | ❌              |
| Asset governance         | ❌       | ❌         | ❌       | ❌              |
| Perspective Preferences  | ✅       | ✅         | ✅       | ✅              |
| Commitment Orchestrator  | ❌       | ❌         | ❌       | ❌              |
| Cluster Orchestrator     | ❌       | ❌         | ❌       | ❌              |

### Air-Gapped environment <a href="#air-gapped-environment" id="air-gapped-environment"></a>

| **Features**             | **AWS** | **Azure** | **GCP** | **Kubernetes** |
| ------------------------ | ------- | --------- | ------- | -------------- |
| Perspectives             | ✅       | ❌         | ❌       | ✅              |
| Cost categories          | ✅       | ❌         | ❌       | ✅              |
| Budgets                  | ✅       | ❌         | ❌       | ✅              |
| BI dashboards            | ✅       | ❌         | ❌       | ✅              |
| Anomaly detection        | ✅       | ❌         | ❌       | ✅              |
| Currency standardization | ❌       | ❌         | ❌       | ❌              |
| Recommendations          | ✅       | ✅         | ✅       | ✅              |
| AutoStopping             | ❌       | ❌         | ❌       | ❌              |
| Asset governance         | ❌       | ❌         | ❌       | ❌              |
| Perspective Preferences  | ✅       | ❌         | ❌       | ✅              |
| Commitment Orchestrator  | ❌       | ❌         | ❌       | ❌              |
| Cluster Orchestrator     | ❌       | ❌         | ❌       | ❌              |

{% hint style="info" %}
* Margin Obfuscation is not supported on Harness SMP. For other environments, it is behind the `CCM_MSP` feature flag. To enable the feature flag in your Harness account, contact [Harness Support](mailto:support@harness.io).
* Istio virtual services are available for Azure in strict mode.
* The cost data for Kubernetes workloads will be derived from the public pricing provided by the respective cloud provider.
* Tracking recommendation lifescyle through Jira and ServiceNow is not supported in Air-gapped environments.
{% endhint %}

### CACM on Air-Gapped Environment <a href="#cacm-on-air-gapped-environment" id="cacm-on-air-gapped-environment"></a>

CACM is supported in [Harness Self-Managed Enterprise Edition installs on an air-gapped environment](https://developer.harness.io/self-managed-enterprise-edition/use-self-managed-enterprise-edition/smp-installationupgrade/helm-installation/install-in-an-air-gapped-environment).

CACM leverages AWS APIs that require connectivity from the isolated (air-gapped) instance. To grant access to these AWS APIs, establish VPC endpoints for the respective AWS services. For services lacking VPC endpoints, use a proxy to facilitate access. For more information, go to [Manage AWS costs by using CCM on Harness Self-Managed Enterprise Edition](../../../../resources/self-managed-enterprise-edition/aws-smp.md).

For a comprehensive list of supported features in other Harness modules and the Harness Platform overall, go to [Supported platforms and technologies](https://developer.harness.io/harness-ai/new-to-harness-platform/platform-whats-supported).
