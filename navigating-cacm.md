---
description: >-
  The three pillars of Cloud & AI Cost Management and how visibility,
  governance, and optimization come together across cloud and AI spend.
tags:
  - cloud-cost-management
---


# Navigating CACM

Cloud & AI Cost Management (CACM) is organized around three pillars: visibility, governance, and optimization. This page explains what each pillar covers and how they come together into a single FinOps workflow across cloud and AI spend.

***

### What CACM offers: the three pillars <a href="#what-cacm-offers-the-three-pillars" id="what-cacm-offers-the-three-pillars"></a>

#### Cost Visibility <a href="#cost-visibility" id="cost-visibility"></a>

* [Cost Explorer](../cost-reporting/cost-explorer.md): Custom views to analyze cloud and AI costs by provider, service, region, tags, or business units. Organize them into folders, share with teams via RBAC, and use the "Ask AI" feature to create views using natural language. Track spending on AI providers like Anthropic and OpenAI, analyze token usage, and understand AI cost growth patterns.
* [Cost Categories](../cost-reporting/cost-categories/): Define custom cost allocation rules to map cloud and AI spending to your business structure. Group resources by team, product, cost center, or any business dimension. The same rules you have written for cloud chargeback now apply to AI spend.
* [BI Dashboards](../cost-reporting/bi-dashboards/): Pre-built and customizable business intelligence dashboards powered by Looker. Visualize cost trends, compare spending across teams or projects, and create executive-level reports. Dashboards can be scheduled, shared, and embedded to keep stakeholders informed without requiring them to log into CACM.

#### Cost Optimization <a href="#cost-optimization" id="cost-optimization"></a>

* [Recommendations](../cost-optimization/recommendations/): AI-powered suggestions to right-size Kubernetes workloads, node pools, EC2 instances, ECS services, and Azure VMs. View potential savings, create Jira/ServiceNow tickets, and track which recommendations have been applied.
* [AutoStopping](../cost-optimization/autostopping-rules/): Automatically stop idle non-production resources based on traffic or schedules. Supports EC2, ECS, RDS, Azure VMs, and GKE clusters. Typically saves 60-70% on non-production costs with zero code changes.
* [Commitment Orchestration](../cost-optimization/commitment-orchestrator/): Automate AWS Reserved Instance and Savings Plan management. Analyze coverage and utilization, track savings vs. on-demand pricing, and let Harness automatically purchase optimal commitments or use manual approval mode.
* [Cluster Orchestrator](../cost-optimization/cluster-orchestrator-for-aws-eks-clusters/): Intelligent Kubernetes cluster management that automatically optimizes node pools, manages spot instances, and handles workload bin-packing. Reduces cluster costs while maintaining performance and availability SLAs.

#### Cost Governance <a href="#cost-governance" id="cost-governance"></a>

* [Budgets](../cost-governance/budgets/): Set daily, weekly, monthly, quarterly, or yearly spending limits tied to Cost Explorer views. Configure multiple alert thresholds and receive notifications via email or Slack before costs exceed budget. Available for both cloud and AI costs.
* [Asset Governance](../cost-governance/asset-governance/): Enforce cloud policies using policy-as-code. Create rules to identify non-compliant resources (untagged, idle, misconfigured), group them into rule sets, and schedule automatic evaluations with alerts for violations.
* [Anomaly Detection](../cost-governance/anomalies/): ML-powered detection of unusual spending patterns with configurable sensitivity, status management (Active/Resolved/Archived), and automated alerts to catch cost spikes early. Available for both cloud and AI costs.

***

### Key capabilities <a href="#key-capabilities" id="key-capabilities"></a>

* **AI cost visibility with unit economics**: CACM tracks spending across AI providers (OpenAI, Anthropic, Gemini) and managed AI services (AWS Bedrock, GCP Vertex AI). It breaks cost down by model, token type, agent, session, and inference, and ties spend to business outcomes such as cost per resolved ticket or cost per completed workflow. Go to [AI Cost Management](ai-cost-management/ai-cost-management.md) to set up trace attribution.
* **Automated optimization**: AutoStopping shuts down idle resources, Cluster Orchestrator right-sizes Kubernetes nodes, and Commitment Orchestration purchases RIs and Savings Plans automatically.
* **Cloud-native infrastructure support**: Native support for Kubernetes, containers, serverless, and AI workloads across AWS, Azure, GCP, and external sources.
* **Governance at scale**: Policy-as-code enforcement detects untagged resources, idle infrastructure, and security misconfigurations across hundreds of accounts before they become costly problems.
* **Workflow integrations**: Create Jira or ServiceNow tickets from recommendations. Receive alerts via Slack or email.

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Cost Categories](../cost-reporting/cost-categories/) to set up cost allocation rules and map spend to your teams, products, or cost centers.
* Go to [Cost Explorer](../cost-reporting/cost-explorer.md) to analyze and break down spend across all connected providers.

{% @harness-feedback/feedback %}
