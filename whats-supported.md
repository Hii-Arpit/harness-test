---
description: Supported platforms and feature support matrix for Harness CACM.
---


# What is supported in Harness CACM

{% include "../.gitbook/includes/__shared/shared/ccm-supported-platforms.md" %}

***

### Supported AI providers and capabilities <a href="#supported-ai-providers-and-capabilities" id="supported-ai-providers-and-capabilities"></a>

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

### Supported environments <a href="#supported-environments" id="supported-environments"></a>

Harness CACM supports the following platforms and orchestration systems:

#### Cloud platforms <a href="#cloud-platforms" id="cloud-platforms"></a>

* AWS
* GCP
* Azure

#### Container Orchestration <a href="#container-orchestration" id="container-orchestration"></a>

* Kubernetes: EKS (AWS), GKE (GCP), AKS (Azure)
* ECS Clusters

#### Deployment model <a href="#deployment-model" id="deployment-model"></a>

* Harness SaaS

{% hint style="info" %}
Go to [Data Sources and Refresh Rates](data-ingestion-reference.md) to review what CACM ingests from each provider, the source, and how often it refreshes.
{% endhint %}

***

#### Supported Kubernetes management platform <a href="#supported-kubernetes-management-platform" id="supported-kubernetes-management-platform"></a>

The following section lists the support for Kubernetes management platform for CACM:

| **Technology**               | **Supported Platform** | **Pricing**      |
| ---------------------------- | ---------------------- | ---------------- |
| OpenShift 3.11               | GCP                    | GCP              |
| OpenShift 4.3                | AWSOn-Prem             | AWSCustom-rate\* |
| Rancher                      | AWS                    | Custom-rate\*\*  |
| Kops (Kubernetes Operations) | AWS                    | AWS              |

* Cost data is supported for On-Prem OpenShift 4.3. This uses a custom rate.
* Cost data is supported for K8s workloads on AWS managed by Rancher, but the cost falls back to the custom rate.

{% @harness-feedback/feedback %}
