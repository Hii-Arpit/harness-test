---
description: Understand the features facilitated by SMP.
---


# On-premises (SMP)

This topic describes features of SMP (Self-Managed Platform).

Feature availability on HCE SaaS and SMP are on par, with minor timeline changes in the SMP feature releases.

The table below outlines the roadmap for the Harness Self-Managed Enterprise Edition of Chaos Engineering:

| **Release version**                | **Feature set**          | **Deployment infrastructure**                                                           | **Supported platforms** | **Supported ingress**                                  |
| ---------------------------------- | ------------------------ | --------------------------------------------------------------------------------------- | ----------------------- | ------------------------------------------------------ |
| Limited GA (Current version)       | Feature parity with SaaS | <ul><li>Cloud</li><li>Connected</li><li>Airgapped</li><li>Signed certificates</li></ul> | Kubernetes clusters     | <ul><li>NGINX</li><li>Istio virtual services</li></ul> |
| General Availability (Coming soon) | Feature parity with SaaS |                                                                                         |                         |                                                        |

{% hint style="info" %}
**NOTE**

To install SMP in an air-gapped environment, go to [SMP in air-gapped environment](https://developer.harness.io/self-managed-enterprise-edition/use-self-managed-enterprise-edition/smp-installationupgrade/helm-installation/install-in-an-air-gapped-environment).
{% endhint %}

For more information, go to What's supported (see documentation).

For information on feature releases, go to [SMP Release Notes](https://app.gitbook.com/s/RdSRdIerXDmqsO6h9KZy/self-managed-enterprise-edition).

## Enterprise ChaosHub <a href="#enterprise-chaoshub" id="enterprise-chaoshub"></a>

Enterprise ChaosHub is now built into the chaos-manager and does not require external GitHub connectivity. The chaos faults, experiment templates, and probe templates are bundled with the SMP installation.

{% hint style="info" %}
For the latest features and updates in SMP, refer to the [SMP Release Notes](https://app.gitbook.com/s/RdSRdIerXDmqsO6h9KZy/self-managed-enterprise-edition).
{% endhint %}

{% @harness-feedback/feedback %}
