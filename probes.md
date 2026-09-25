---
description: >-
  Use probes to validate system health and define success criteria for chaos
  experiments
---


# Probes

Probes are validation mechanisms that monitor and verify the health of your system throughout chaos experiments. They act as automated checkpoints that determine whether your system maintains expected behavior under failure conditions.

## How Probes Affect Resilience Score <a href="#how-probes-affect-resilience-score" id="how-probes-affect-resilience-score"></a>

Each probe in an experiment contributes to the overall **resilience score**. When a probe executes, it results in either a **PASS** or **FAIL** state:

* **PASS** - The system met the expected criteria (e.g., HTTP 200 response, metric within threshold)
* **FAIL** - The system did not meet the expected criteria

The resilience score is calculated as a weighted average of all probe and fault outcomes in the experiment. A higher pass rate across probes means a higher resilience score, giving you a quantifiable measure of your system's ability to withstand failures.

## Probe Types <a href="#probe-types" id="probe-types"></a>

Harness supports the following probe types. To create a probe, navigate to **Project Settings** > **Chaos Probes** and click **+ New Probe**. Select your infrastructure type, then choose from:

* [**HTTP Probe**](http-probe.md) - Validate service endpoints by checking HTTP response codes or body content
* [**Command Probe**](command-probe.md) - Run shell commands and match output for custom validation logic
* [**Container Probe**](container-probe.md) - Execute commands inside a container with full control over image, volumes, and security context
* [**APM Probe**](apm-probes/) - Query metrics from monitoring systems. Select **APM Probe**, then choose the provider: Prometheus, Datadog, Dynatrace, New Relic, Splunk Observability, Splunk Enterprise, AppDynamics, or GCP Cloud Monitoring

{% hint style="warning" %}
**Deprecated probe types**

The following probe types are no longer available as top-level options in the probe picker:

* **K8s Probe:** Use a [Command Probe](command-probe.md), or a built-in [Kubernetes command probe template](probe-template-library/kubernetes/index.md), for resource-state checks.
* **Prometheus Probe, Datadog Probe, and Dynatrace Probe:** Create an [APM Probe](apm-probes/) and select the provider in the probe wizard.
{% endhint %}

All probes follow the same creation wizard: **Overview** → **Variables** → **Probe Properties** → **Run Properties**. The probe-specific configuration happens in the Probe Properties step; everything else is common across all probe types. See the individual probe pages for step-by-step details.

## Infrastructure Support <a href="#infrastructure-support" id="infrastructure-support"></a>

Not all probes are available on every infrastructure type. The following table shows which probes are supported on each infrastructure:

| Probe                                 | Kubernetes | Linux | Windows |
| ------------------------------------- | :--------: | :---: | :-----: |
| [HTTP Probe](http-probe.md)           |      ✅     |   ✅   |    ✅    |
| [Command Probe](command-probe.md)     |      ✅     |   ✅   |    ✅    |
| [Container Probe](container-probe.md) |      ✅     |   -   |    -    |
| [APM Probe](apm-probes/)              |      ✅     |   -   |    -    |

The table shows native (inline) support on each infrastructure. Probes that are native to Kubernetes only, such as the Container Probe and APM probes, can also run in Linux and Windows experiments through remote Kubernetes execution. Go to [Remote Kubernetes Execution](./#remote-kubernetes-execution) to understand how this works.

## Remote Kubernetes Execution <a href="#remote-kubernetes-execution" id="remote-kubernetes-execution"></a>

When you run an experiment on a Linux or Windows infrastructure, you can also run Kubernetes probes by mapping a Kubernetes infrastructure to the experiment. Linux and Windows probes run inline on the target machine, while the mapped Kubernetes probes run remotely on the mapped Kubernetes infrastructure.

This lets you use Kubernetes-only probes, such as the Container Probe and APM probes, alongside your Linux and Windows experiments without recreating them for each infrastructure.

You map a Kubernetes infrastructure in one of two ways:

* When you set up the Linux or Windows infrastructure, where the mapped Kubernetes infrastructure applies to experiments on that infrastructure.
* When you add a probe to an experiment, where you map a Kubernetes infrastructure to that probe so it runs as a remote probe.

Remote Kubernetes execution gives you the following benefits:

* **Reuse across infrastructures:** Use the same probe definition and logic on Kubernetes, Linux, and Windows infrastructures.
* **Access to Kubernetes-only capabilities:** Use features such as external secrets in Linux and Windows experiments.
* **Less duplicate maintenance:** Keep a single probe definition instead of maintaining separate probes per infrastructure.

## Built-in Probe Templates <a href="#built-in-probe-templates" id="built-in-probe-templates"></a>

Harness provides [built-in probe templates](probe-templates.md) to help you quickly set up probes for common validation scenarios. These templates are Command Probes that run on Kubernetes chaos infrastructure, covering Kubernetes resource checks (pod status, node health, resource utilisation, and more) as well as AWS and GCP resource checks (EC2, ECS, Lambda, load balancers, Compute Engine VMs, persistent disks, and Cloud SQL).

## Probe Verification <a href="#probe-verification" id="probe-verification"></a>

Probes can be marked as **Verified** to ensure only tested and approved probes are used in production experiments. This governance feature helps teams maintain quality standards and prevent misconfigurations.

To mark a probe as verified:

1. Navigate to **Project Settings** > **Chaos Probes**
2. Click the three-dot menu (⋮) next to the probe
3.  Select **Mark as Verified**

    ![Mark as Verified](../../.gitbook/assets/mark-as-verified.png)

Once verified, the probe displays a green checkmark (✓) in the **Verification Status** column. You can then use [ChaosGuard](../../shared-capabilities/governance/governance-in-execution/govern-run.md) policies to mandate that only verified probes can run in experiments.

## Next Steps <a href="#next-steps" id="next-steps"></a>

* [Create Chaos Experiments](../experiments/) - Build experiments with probes
* [ChaosHub](../chaoshub/) - Discover and share experiment templates
* [ChaosGuard](../../shared-capabilities/governance/governance-in-execution/govern-run.md) - Enforce governance policies on probe usage

{% @harness-feedback/feedback %}
