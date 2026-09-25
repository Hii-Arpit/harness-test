---
description: Choose between SaaS and Self-Managed Platform deployments
---


# Deployment Options

Harness Resilience Testing can be deployed in two ways to meet your organization's needs.

## SaaS <a href="#saas" id="saas"></a>

**Fully managed cloud service** hosted by Harness with automatic updates and scaling.

### Benefits <a href="#benefits" id="benefits"></a>

* Quick setup with minimal configuration
* Automatic updates and maintenance
* Scalable infrastructure managed by Harness
* Access from anywhere with internet connectivity

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Harness account
* Network connectivity to Harness SaaS endpoints
* Appropriate firewall rules for outbound HTTPS (port 443)

### Getting Started <a href="#getting-started" id="getting-started"></a>

1. [Sign up for Harness](https://app.harness.io/auth/#/signup)
2. Create your first project
3. [Install chaos infrastructure](../chaos-testing/infrastructure/)
4. [Run your first chaos experiment](../chaos-testing/get-started.md)

## On-Premise (Self-Managed Platform) <a href="#on-premise-self-managed-platform" id="on-premise-self-managed-platform"></a>

**Deploy in your own infrastructure** for complete control over data and compliance.

### Benefits <a href="#benefits" id="benefits"></a>

* Full control over deployment environment
* Data remains within your infrastructure
* Custom security policies and compliance
* Air-gapped deployment support

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Kubernetes cluster for Harness Platform
* Sufficient resources for control plane components
* Network connectivity between components
* Valid Harness license

### Getting Started <a href="#getting-started" id="getting-started"></a>

1. Review [SMP installation guide](https://developer.harness.io/self-managed-enterprise-edition/use-self-managed-enterprise-edition/smp-installationupgrade/helm-installation/install-using-helm)
2. Install Harness Platform in your environment
3. Configure [chaos infrastructure](../chaos-testing/infrastructure/)
4. [Run your first chaos experiment](../chaos-testing/get-started.md)

For more information about SMP, see [Self-Managed Platform documentation](../shared-capabilities/on-premises-smp/).

## Comparison <a href="#comparison" id="comparison"></a>

| Feature                | SaaS               | Self-Managed Platform |
| ---------------------- | ------------------ | --------------------- |
| **Setup Time**         | Minutes            | Hours to Days         |
| **Maintenance**        | Managed by Harness | Self-managed          |
| **Updates**            | Automatic          | Manual                |
| **Data Location**      | Harness Cloud      | Your Infrastructure   |
| **Customization**      | Standard           | Full Control          |
| **Air-gapped Support** | No                 | Yes                   |

## Next Steps <a href="#next-steps" id="next-steps"></a>

* [Understand key concepts](../new-to-resilience-testing/key-concepts.md)
* [Learn about architecture](../new-to-resilience-testing/architecture.md)
* [Explore chaos testing features](../chaos-testing/get-started.md)

{% @harness-feedback/feedback %}
