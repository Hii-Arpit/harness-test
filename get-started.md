---
description: >-
  Create a disaster recovery test from Resilience Testing, open it in Pipeline
  Studio, and run a probe-fault-probe recovery workflow.
tags:
  - disaster-recovery
  - resilience-testing
---


# Get Started with DR Testing

Disaster Recovery (DR) Testing validates that your systems can recover from catastrophic failures. Each DR test is a Harness pipeline with a **Disaster Recovery** stage (`DRTest`), so you orchestrate failover, validation, and notification in a repeatable, auditable workflow.

You start from **Resilience Testing** > **DR Tests**. Harness creates the pipeline, then hands you into Pipeline Studio to build the recovery steps.

{% hint style="info" %}
**FEATURE FLAG**

DR Testing is currently behind a feature flag (`CHAOS_DR_TESTING_ENABLED`). Contact your Harness sales representative to get it enabled for your account.
{% endhint %}

***

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **DR Tests access:** View and Create / Edit permissions on DR Tests. Go to [RBAC in Resilience Testing](../shared-capabilities/rbac.md) to configure roles.
* **A Kubernetes chaos infrastructure:** Chaos Fault, Chaos Probe, and Chaos Action steps each select an infrastructure in the form `<environment>/<infrastructure>`, so you need at least one connected before the steps will validate. Go to [Set up Kubernetes infrastructure](../chaos-testing/infrastructure/kubernetes/) to connect one, which also creates the Harness [environment](https://developer.harness.io/continuous-delivery/use-continuous-delivery/cd-building-blocks/environments/create-environments) it belongs to.
* **Pipeline permissions:** View, Create / Edit, and Execute on pipelines so you can open Pipeline Studio and run the test.

***

## Review the DR Tests list <a href="#review-the-dr-tests-list" id="review-the-dr-tests-list"></a>

Go to **Resilience Testing** > **DR Tests**. The list shows pipelines that contain a Disaster Recovery stage.

| Column                | What it shows                                                           |
| --------------------- | ----------------------------------------------------------------------- |
| **Pipeline name**     | The pipeline display name and identifier.                               |
| **DR test stages**    | The Disaster Recovery stage names inside that pipeline.                 |
| **Recent executions** | Recent run statuses for the pipeline.                                   |
| **Last execution**    | Status and time of the most recent run, or **Never** if it has not run. |
| **Last modified**     | Who last saved the pipeline and when.                                   |

Use **Search** and **Tag(s)** to narrow the list. **Reset** clears active filters. The sort control defaults to **Last Modified (New -> Old)**.

![The DR Tests list, with one row per pipeline that contains a Disaster Recovery stage](../.gitbook/assets/dr-tests-list.png)

Select a pipeline name to open it in Pipeline Studio. Recent execution links open that run in the Continuous Delivery execution view.

***

## Create your first DR test <a href="#create-your-first-dr-test" id="create-your-first-dr-test"></a>

### Enter the details <a href="#enter-the-details" id="enter-the-details"></a>

1. Go to **Resilience Testing** > **DR Tests**.
2. Click **+ New DR Test**.
3. In **Create new DR Test**, fill in the **DR Test Details**:

| Field           | Description                                                        |
| --------------- | ------------------------------------------------------------------ |
| **Name**        | Display name for the DR stage. Harness derives the **Id** from it. |
| **Description** | Optional details about the disaster scenario.                      |
| **Tags**        | Optional labels for filtering.                                     |
| **Objective**   | Optional goal or success criteria for the test.                    |

**Description** and **Tags** are collapsed to a label and a pencil icon until you select the pencil, so an empty dialog shows only **Name** and **Objective** as open fields.

![The Create new DR Test dialog, with the Id derived from the name](../.gitbook/assets/dr-create-dialog.png)

4. Click **Continue in Pipeline Studio**.

Harness creates a pipeline (named `<Name> Pipeline`), adds a stage of type `DRTest` with the details you entered, tags the pipeline with `module: drtest`, and opens Pipeline Studio in the Continuous Delivery module.

### Configure the stage <a href="#configure-the-stage" id="configure-the-stage"></a>

Pipeline Studio shows three tabs on the Disaster Recovery stage: **Overview**, **Execution**, and **Advanced**.

1. On **Overview**, confirm the stage **Name**, **Id**, **Description**, **Tags**, and **Objective**. Expand **Advanced** to set a stage **Timeout** or **Stage Variables** if you need them. Click **Continue**.

{% hint style="info" %}
**THERE IS NO ENVIRONMENT TAB**

A Disaster Recovery stage does not select an environment or an infrastructure at stage level. Each Chaos Fault, Chaos Probe, and Chaos Action step selects its own target as `<environment>/<infrastructure>`, so two steps in the same stage can act on different infrastructures. Stage-level failure handling lives on the **Advanced** tab.
{% endhint %}

2. On **Execution**, build the recovery workflow. Click **Add Step**, then select **Add Step** from the menu to open the **Step Library**. Under **Resilience Testing**, add the steps you need:

| Step type             | What it does                                                                                                                                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Chaos Probe**       | Validates a condition against your system, such as pod health or an HTTP response. Use before and after a fault to verify baseline and recovery. |
| **Chaos Fault**       | Injects a failure, such as pod delete or network loss, to simulate the disaster.                                                                 |
| **Chaos Action**      | Runs a predefined chaos action from Resilience Testing.                                                                                          |
| **Chaos Experiment**  | Runs an existing chaos experiment as a single step.                                                                                              |
| **Load Test**         | Runs a load test, so you can hold traffic on the system while the failover happens. Go to [Load testing](../load-testing/get-started.md) to build one. |

![The Resilience Testing group in the Step Library](../.gitbook/assets/dr-step-library.png)

The same menu also offers **Add Step Group**, **Use template** (insert a step or step group template), and **Create with AI**. Under **Approval**, you can add **Harness Approval** or other approval steps when a human gate belongs in the workflow. An empty Disaster Recovery stage can be saved; Harness does not require approval, fault, and probe steps to be present before save.

A typical first workflow follows **Probe → Fault → Probe**:

1. **Chaos Probe** to confirm baseline health.
2. **Chaos Fault** to inject the failure.
3. **Chaos Probe** to confirm the system recovers.

You can also add standard Harness steps (shell script, HTTP, notification) alongside Resilience Testing and Approval steps.

For each Chaos Probe, Chaos Fault, or Chaos Action step:

1. Select the probe, fault, or action to run. The picker is named after the step: **Resilience Test Probe**, **Chaos Fault**, or **Resilience Test Action**.
2. Under **Select RT Infrastructure**, select the target as `<environment>/<infrastructure>`.
3. Set **Duration** when the step requires it. Chaos Fault does not take one; the fault carries its own.
4. On a Chaos Probe, optionally select a **Chaos Service** to attribute the result to a service you have onboarded. Go to [Services](../shared-capabilities/services/) to onboard one.
5. Fill any **Runtime Inputs** the selected resource exposes. Pin a field when you want Harness to prompt for the value at run time.
6. Click **Apply Changes**.

![The Chaos Probe step drawer, with the probe, infrastructure, duration, and service fields](../.gitbook/assets/dr-chaos-probe-step.png)

To reuse a configured step later, click **Save as Template** on the step drawer. To insert an existing template, use **Use template** from the Add Step menu and pick a step or step group template.

Open **Variables** in Pipeline Studio to add pipeline-level or stage-level variables, and reference them from step inputs with Harness expressions.

Go to [Pipeline Stage Reference](pipeline-stage-reference.md) for the full field list.

### Use the Rollback path <a href="#use-the-rollback-path" id="use-the-rollback-path"></a>

The Execution tab offers an **Execution | Rollback** toggle. **Execution** is the forward recovery workflow. **Rollback** is the path Harness can run when a failure strategy selects **Rollback Stage** or **Rollback Pipeline**. Add compensating steps on the Rollback canvas the same way you add Execution steps. Configure the failure strategy on the **Advanced** tab to invoke rollback; Harness does not automatically switch to Rollback solely because a probe fails unless your failure strategy says so.

### Save and run <a href="#save-and-run" id="save-and-run"></a>

1. Click **Save** to persist the pipeline.
2. Click **Run** to execute it.
3. Monitor progress in Pipeline Studio, then open **Execution History** for past runs.

***

## Add a Disaster Recovery stage to an existing pipeline <a href="#add-a-disaster-recovery-stage-to-an-existing-pipeline" id="add-a-disaster-recovery-stage-to-an-existing-pipeline"></a>

You do not have to start from **+ New DR Test**. From any pipeline in Pipeline Studio:

1. Click **Add Stage**.
2. Select **Disaster Recovery**.
3. Configure Overview and Execution as above.

This is the same stage type the DR Tests entrypoint creates. Pipelines that contain it appear on the **DR Tests** list.

***

## Review probe results after a run <a href="#review-probe-results-after-a-run" id="review-probe-results-after-a-run"></a>

Open a pipeline execution and select the **Resilience Tests** tab. The tab lists the Resilience Testing steps from the run and surfaces **Probe Results** for probe outcomes. Use step **Details**, **Step Logs**, and **Console View** for Chaos Fault and Chaos Action failures. The tab is not a fault catalog.

***

## Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Pipeline Stage Reference](pipeline-stage-reference.md) for the complete field list for the create dialog, the stage tabs, and the DR steps.
* Go to [Concepts](concepts.md) to understand RTO, RPO, and failure strategies.
* Go to [Chaos Testing](../chaos-testing/get-started.md) to combine DR testing with chaos experiments and services.

{% @harness-feedback/feedback %}
