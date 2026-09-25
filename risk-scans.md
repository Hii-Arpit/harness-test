---
description: >-
  Run a pipeline scan or an infrastructure scan in Harness Resilience Testing to
  detect risks and generate a risk report.
tags:
  - chaos-engineering
  - risks
---


# Risk Scans

A risk scan reads the manifests behind your applications, matches them against the Harness resilience rules, and produces a report of the risks it found. Run a scan when you want to know where your system is fragile before you spend time designing experiments.

You also get a scan during [service onboarding](../services/service-discovery.md#scanning-stage), as the middle stage of the discovery → scanning → onboarding flow. This page covers the on-demand pipeline and infrastructure scans you start from Insights. Everything after collection, including the analysis, the scoring, and the report, is identical regardless of how the scan started.

{% hint style="info" %}
**FEATURE FLAG**

Risk scans are currently behind a feature flag (`CHAOS_RESILIENCE_RISKS_ENABLED`). Contact your Harness sales representative to get it enabled for your account.
{% endhint %}

***

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **A scan source:** Either a Harness pipeline with a deploy stage, or an infrastructure with a discovery agent and a connector that reaches the cluster. Go to [Set up Kubernetes infrastructure](../../chaos-testing/infrastructure/kubernetes/) to connect an infrastructure.
* **Risk concepts:** Go to [Risks](./) to understand risk rules, severity, and the difference between a passive and a confirmed risk.
* **Project access:** Permissions to view and create resilience testing resources in the project. Go to [RBAC in Harness](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control) to configure roles.

***

## How a scan runs <a href="#how-a-scan-runs" id="how-a-scan-runs"></a>

Every scan moves through three stages in order, and the status tells you which stage it is in.

1. **Collection:** Harness gathers the application manifests from the source. This is the only stage that differs between the two scan kinds.
2. **Analysis:** Harness matches the collected manifests against the risk rules and produces risks with severities attached.
3. **Reporting:** Harness assembles the findings into a downloadable risk report.

The status moves through **Pending**, **Collecting**, **Analyzing**, and **Reporting** before it reaches **Completed**. A scan that fails reports **Errored**, and one that is stopped reports **Aborted**. An errored scan can still show partial results, so treat its counts as incomplete rather than final.

***

## Run a scan <a href="#run-a-scan" id="run-a-scan"></a>

Choose the scan kind that matches the source you want to read from.

{% tabs %}
{% tab title="Pipeline Scan" %}
A pipeline scan reads the application definition from a pipeline's deploy stage. Use it to catch risks in what a pipeline is about to deploy, before it reaches a cluster.

1. Go to **Resilience Testing → Insights → Pipeline Scans**.
2. Select **+ New Scan**. The **Scan a CD Pipeline** dialog lists the candidate pipelines with their recent executions, who last executed each one, and when it was last modified.
3. Find the pipeline to scan. Use **Search for a CD pipeline** to narrow the list.
4. Select **Scan now** on that row, or open the dropdown beside it to choose between a **Rule-based Scan** and an **AI Scan**. Go to [Choose an analyzer](risk-scans.md#choose-an-analyzer) to decide which one to use.

The scan starts as soon as you choose, and the new run appears at the top of the list.

{% hint style="info" %}
**A PIPELINE CANNOT BE SELECTED**

A pipeline needs a deploy stage and at least one successful execution, because the manifests Harness reads come from that execution. A pipeline that has never deployed successfully is listed but grayed out, and hovering it reports **No successful executions found**.
{% endhint %}

Pipeline scans are available on Harness SaaS only. On Self-Managed Enterprise Edition, use an infrastructure scan instead. Go to [Risks](./#availability-on-self-managed-enterprise-edition) to review what is supported where.
{% endtab %}

{% tab title="Infrastructure Scan" %}
An infrastructure scan reads the applications a discovery agent already found in a cluster. Use it to assess what is running now rather than what is about to be deployed.

1. Go to **Resilience Testing → Insights → Infrastructure Scans**.
2. Select **+ New Scan**. The **Scan a Kubernetes Infrastructure** dialog lists every discovery agent with the number of resources it found and the time of its last discovery.
3. Find the discovery agent to scan. Each scan maps to one discovery agent, and therefore to one infrastructure.
4. Select **Scan now** on that row, or open the dropdown beside it to choose between a **Rule-based Scan** and an **AI Scan**. Go to [Choose an analyzer](risk-scans.md#choose-an-analyzer) to decide which one to use.

The scan starts as soon as you choose, and the new run appears at the top of the list.

Because the source is a discovery agent, the scan covers whatever that agent discovered on its last sweep. Go to [Customize discovery agent](https://developer.harness.io/harness-platform/use-harness-platform/service-discovery/customize-agent) to change the namespaces or schedule the agent uses.
{% endtab %}
{% endtabs %}

***

## Choose an analyzer <a href="#choose-an-analyzer" id="choose-an-analyzer"></a>

Both scan kinds offer two analyzers, chosen from the dropdown beside **Scan now**. The collection and reporting stages are the same either way, and only the analysis differs.

| Analyzer            | Behavior                                                                                                                                              |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rule-based Scan** | Matches manifests against the risk rules with deterministic logic. Faster, and repeatable across runs.                                                |
| **AI Scan**         | Passes the manifests and the risk rules to an AI agent, which reasons about which risks apply. Slower, and better at context that fixed logic misses. |

Start with the rule based analyzer when you want quick, repeatable results in a pipeline. Use the AI analyzer when you want a closer reading of an application you are about to take to production.

The analyzer a scan used is recorded against it, so the **Scan Type** column shows a **Rule-based** or **AI** label on every row and you can tell which reading produced which result.

***

## Review a completed scan <a href="#review-a-completed-scan" id="review-a-completed-scan"></a>

Select a scan to open it.

![Completed pipeline scan with the About this Scan rail on the left, the Pipeline Risk Score, Services Scanned, and Risks Detected cards across the top, and the Risk Detection Heatmap and Services with risk sections below](../../.gitbook/assets/risk-scan-review.png)

**About this Scan** in the left rail holds the date of the scan and its status. A pipeline scan also names the **Pipeline** it read from and the **Pipeline Execution** the manifests came from, and an infrastructure scan names the **Environment** and the **Infrastructure**, all as links back to the source.

The score card is labeled **Pipeline Risk Score** or **Infrastructure Risk Score** to track the scan type, and reports out of 1000. Go to [Risks](./#severity-and-risk-score) to understand how it is calculated.

Three sections follow the cards.

* **Risk Detection Heatmap:** Services against risk rules, with each cell colored by severity. Go to [Risks](./#the-risk-heat-map) to read it.
* **Services with risk:** Every affected service, sorted from highest to lowest severity composition, with a stacked **Risk Composition** bar per service.
* **Risks Detected:** The full list of risks, with a **Severity**, **Risk**, **Service**, and **Recommendation** column. Search it by risk name or sort by most recently detected.

***

## Download the risk report <a href="#download-the-risk-report" id="download-the-risk-report"></a>

A scan that reaches **Completed** offers a **PDF Report** button in the header of the scan page. Scans that errored or were aborted have no report to download.

The report is titled **Passive Risk Detection Report** and runs to nine pages. Its sections are numbered and move from summary to detail.

| Section                                                 | Contents                                                                                                                                                                                                                                             |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Executive Summary**                                   | The risk score, the counts by severity, a written overview of what the scan found, and a **Scanned via** table naming the scan type, organization, project, infrastructure, environment, and timestamp.                                              |
| **Services Found in Infra**                             | Each service with its Resilience Risk Score and its counts by severity, plus whether the score sits above or below the recommended gate threshold of 500. The heading tracks the scan type, so a pipeline scan reads **Services Found in Pipeline**. |
| **Risks Found**                                         | One card per risk, with its severity, a description of the weakness, and the services it affects.                                                                                                                                                    |
| **Risk Heatmap, Service by Risk Type**                  | Risk count per service per severity, where darker cells indicate higher concentration and the least severe rule columns are grouped into **Other**.                                                                                                  |
| **Risk Composition by Service**                         | A doughnut chart per category, so you can see the split across Availability, Performance, Resilience, and Config at a glance.                                                                                                                        |
| **Recommendations & Next Steps**                        | The top services by Resilience Risk Score, with a **What we found** column and a **Recommended fix** column.                                                                                                                                         |
| **How Harness Chaos Engineering Validates These Fixes** | The chaos experiments that verify the recommended fixes.                                                                                                                                                                                             |
| **Appendix, About This Report & Glossary**              | How the scan was run, what it did and did not assess, and a glossary of the terms used, including RRS, HPA, PDB, and QoS.                                                                                                                            |

***

## What a scan does not do <a href="#what-a-scan-does-not-do" id="what-a-scan-does-not-do"></a>

Scans are on-demand and single-source by design. Plan around the following limits.

* **One source per scan:** A scan targets a single pipeline or a single discovery agent. You cannot select several pipelines or several infrastructures in one scan.
* **No scheduled scans:** Scans do not run on a recurring schedule. Start each one yourself.
* **No trigger-based scans:** Scans cannot be started by an event or a pipeline trigger.

To keep risk data current, re-run the scan after a deployment that changes the manifests it read.

***

## Find an earlier scan <a href="#find-an-earlier-scan" id="find-an-earlier-scan"></a>

Both scan lists show every scan of that kind, one row per run, with these columns.

| Column                         | Contents                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------- |
| **Pipeline Scan**              | The source it read from, and the scan ID. The heading tracks the scan type.     |
| **Scan Type**                  | Whether it ran as **Rule-based** or **AI**.                                     |
| **Risk Score**                 | The score out of 1000, with a bar.                                              |
| **Risks Detected**             | The total, and the counts for Critical, High, Medium, and Low.                  |
| **Status**                     | **Completed**, **Errored**, or **Aborted**.                                     |
| **Infrastructures Onboarded** | How many of the scanned infrastructures are onboarded, with a link to the list. |

Filter by source, which is the **Pipeline** dropdown for a pipeline scan and the **Discovery Agent** dropdown for an infrastructure scan, and filter by **Status** to separate completed runs from the rest. **Reset** clears the filters, and search narrows the list by source name.

The row menu on each scan offers three actions. **JSON** opens the raw scan output, which is useful when you want the findings in a machine-readable form rather than the PDF. **Retry** runs the scan again against the same source, and is enabled only for a scan that ended in an error state. **Delete** removes the scan and its findings.

Because a scan is a point-in-time reading, the list doubles as a history. Compare two scans of the same source to see whether a mitigation actually reduced the risk count.

***

## Confirm the risks a scan found <a href="#confirm-the-risks-a-scan-found" id="confirm-the-risks-a-scan-found"></a>

A scan leaves every risk it finds in the **passive** state, which means the risk is detected but unproven. Confirm the ones that matter.

1. Review the risks by severity, starting with critical.
2. Associate the matching risk rule with a probe, in the **Risk Rule** step of the probe wizard under **Project Settings → Probes**.
3. Add that probe to a chaos experiment that targets the affected service.
4. Run the experiment. A probe failure moves the risk from **Passive** to **Confirmed**.

Go to [Risks](./#passive-and-confirmed-risks) to understand the states in full.

***

## Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

<details>

<summary>A Harness pipeline cannot be selected when creating a new pipeline scan in Resilience Testing</summary>

A pipeline needs a deploy stage and at least one successful execution, because that execution supplies the application manifests. A pipeline that has never deployed successfully is grayed out with **No successful executions found**. Run the pipeline to a successful deployment, add a deploy stage if it has none, or run an infrastructure scan against the cluster instead.

</details>

<details>

<summary>A Resilience Testing risk scan stays in the Collecting stage and never reaches Analyzing</summary>

Collection depends on the scan source. For an infrastructure scan, confirm the discovery agent is connected and has completed a sweep. For a pipeline scan, confirm the deploy stage resolves its manifests successfully.

</details>

<details>

<summary>A Resilience Testing infrastructure scan completes but returns far fewer risks than expected</summary>

An infrastructure scan only covers what the discovery agent found on its last sweep, and not every risk rule applies to every application. Confirm the agent covers the namespaces you expect, then re-run the scan.

</details>

***

## Next steps <a href="#next-steps" id="next-steps"></a>

* [Risks](./): Understand risk rules, scoring, and the passive to confirmed lifecycle.
* [Probes](../../chaos-testing/probes/): Build the checks that confirm a risk.
* [Chaos experiments](../../chaos-testing/experiments/): Run a fault to verify a finding against a live system.

{% @harness-feedback/feedback %}
