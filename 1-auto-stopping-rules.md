---
description: >-
  AutoStopping Rules make sure that your non-production resources run only when
  used, and never when idle.
---


# Overview

## What are AutoStopping rules? <a href="#what-are-autostopping-rules" id="what-are-autostopping-rules"></a>

Cloud resources in non-production environments (dev, test, staging) are typically used for only 30-40% of the time they are running, yet you pay for 100% of that time. AutoStopping Rules ensure you are only **paying for resources when they are actually in use**.

AutoStopping Rules let you define:

* **Controlled resources**: Choose exactly which EC2 instances, Auto Scaling Groups, Kubernetes namespaces, RDS databases, or other supported assets the rule should manage.
* **Idle-time threshold**: Specify how long a resource must remain inactive before AutoStopping considers it idle and eligible for shutdown.
* **Optional fixed schedules**: Keep resources online during business hours, maintenance windows, or any recurring schedule you choose.
* **Dependency order**: List upstream and downstream services so AutoStopping stops and starts them in the right sequence.
* **Access method**: Select load balancer, proxy, or direct IP so your teams continue to use the same DNS, SSH, or RDP endpoints without interruption.
* **Set-and-forget automation**: Zero manual overhead after initial setup

***

## What is supported? <a href="#whats-supported" id="whats-supported"></a>

| Cloud Provider | Supported Resources |
| -------------- | ------------------- |
| <img src="../../.gitbook/assets/aws-logo.svg" alt="AWS" width="40"> **AWS** | <ul><li>EC2</li><li>Auto Scaling Groups</li><li>Kubernetes (EKS)</li><li>ECS Services</li><li>RDS Instances</li></ul> |
| <img src="../../.gitbook/assets/azure-logo.svg" alt="Azure" width="40"> **Azure** | <ul><li>On-demand VMs</li><li>Kubernetes Clusters (AKS)</li></ul> |
| <img src="../../.gitbook/assets/gcp-logo.svg" alt="GCP" width="40"> **GCP** | <ul><li>GCE VMs</li><li>Kubernetes Clusters (GKE)</li><li>Instance Groups</li></ul> |

***

## How AutoStopping works <a href="#how-autostopping-works" id="how-autostopping-works"></a>

AutoStopping intelligently manages your cloud resources through a seamless four-step process:

1. **Intelligent Detection**: Resources are continuously monitored for user activity and workload patterns
2. **Automatic Shutdown**: Once your configured idle threshold is reached, resources are safely stopped to eliminate wasted spend
3. **Seamless Restart**: When access is requested, resources are automatically restarted based on your dependency configuration
4. **Transparent Access**: End users continue using the same familiar endpoints (DNS, SSH, RDP) with minimal interruption

AutoStopping offers:

* **Unlimited rules and resources**: Create as many rules as needed with no restrictions
* **Unlimited dependencies**: No limit on the number of dependencies a rule can manage
* **Flexible configuration**: Customize idle time thresholds, schedules, and access methods
* **Resource-specific restart times**:
  * RDS instances: Warm-up time depends on cluster/instance size (\~25 minutes)
  * Other resources: Typically warm up in under 2 minutes (max 5 minutes)

***

## Ready to get started? <a href="#ready-to-get-started" id="ready-to-get-started"></a>

Follow our [Set Up AutoStopping Rules](autostopping-rules/) to set up AutoStopping in your environment in three simple step.

{% @harness-feedback/feedback %}
