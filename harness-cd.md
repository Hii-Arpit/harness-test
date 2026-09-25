---
description: Integrate HCE with CD
---


# Use HCE with Continuous Delivery

You can add chaos experiments to [Harness Continuous Delivery (CD)](https://harness.io/products/continuous-delivery) pipelines as part of your deployment process. This ensures that you validate your system resiliency with every new deployment. Some benefits include:

* Uncovering resilience unknowns
* Increasing developer efficiency
* Reducing resilience debt

You might use chaos experiments in CD pipelines to validate deployments against:

* Existing resilience conditions
* Newly added resilience conditions
* Changes to the platform on which the target deployments run
* Production incidents and alerts
* Configuration changes

Learn more about these benefits in our [CE/CD integration blog](https://www.harness.io/blog/chaos-experiments-in-harness-cd-pipelines).

## Examples of chaos faults to use in your CD pipeline <a href="#examples-of-chaos-faults-to-use-in-your-cd-pipeline" id="examples-of-chaos-faults-to-use-in-your-cd-pipeline"></a>

You might consider injecting these types of faults as chaos steps in your CD pipeline:

* **Network chaos faults** can be used for verifying service or microservice dependencies on each other when there's latency, or when one of the microservices is down. The pod network latency fault is an example.
* **Stress chaos faults** can be used for verifying how microservices behave when there is a noisy neighbor. The pod CPU hog fault is an example.
* **HTTP chaos faults** can be used for verifying how services or APIs behave when one of the APIs is under chaos. The pod HTTP latency fault is an example.

For more information about Harness CD, go to the [CD tutorials](https://developer.harness.io/continuous-delivery/troubleshooting-and-resources/tutorials/cd-gitops-tutorials).

## Use HCE with CD in a chaos experiment/s <a href="#use-hce-with-cd-in-a-chaos-experiments" id="use-hce-with-cd-in-a-chaos-experiments"></a>

To use CE with CD using a selected experiment (though you can use many experiments), you need to:

1. [Add a chaos experiment](harness-cd.md#step-1-add-a-chaos-experiment-and-run-it) and run it to make sure it completes.
2. [Add this experiment to the CD pipeline](harness-cd.md#step-2-add-a-chaos-experiment-to-a-cd-pipeline) as a chaos step.
3. [Choose a failure strategy](harness-cd.md#step-3-choose-a-failure-strategy).

### Step 1: Add a chaos experiment and run it <a href="#step-1-add-a-chaos-experiment-and-run-it" id="step-1-add-a-chaos-experiment-and-run-it"></a>

* Create a chaos experiment and run it to make sure it runs to completion.
*   In the example below, the relevant probes are added to avoid a false positive or false negative scenario around the resilience score.

    ![Completed chaos experiments with probes shown](../../../.gitbook/assets/exp-run-complete-with-probes.png)

### Step 2: Add a chaos experiment to a CD pipeline <a href="#step-2-add-a-chaos-experiment-to-a-cd-pipeline" id="step-2-add-a-chaos-experiment-to-a-cd-pipeline"></a>

Pipelines are organized into stages, each of which handles a major segment of the pipeline process. There are several types of stages available, and you can add chaos experiments as steps in these three stage types:

* Feature Flag
* Deploy
* Custom Stage

**Add a chaos experiment as a step in your CD pipeline**

1. In your Harness project, select **Deployments > Pipelines**, and then select the pipeline where you want to add a chaos experiment.
2.  In the selected pipeline, select **Add Stage**, and then select a stage type.

    Chaos steps are available for **Feature Flag**, **Deploy**, and **Custom Stage** types.

    ![Select a stage screen with Feature Flag, Deploy, and Custom Stage highlighted](../../../.gitbook/assets/pipeline-add-stage.png)
3. Enter a **Stage Name** (and Deployment Type if applicable), and then select **Set Up Stage**.
4.  With the stage you want selected, select **Add Step > Add Step**.

    ![Add step icon with stage selected](../../../.gitbook/assets/pipeline-add-step.png)

    The Step Library appears.

    ![The Step Library showing a few of many available steps](../../../.gitbook/assets/pipeline-step-library.png)
5.  Scroll down the list to find the Chaos step icon, and then select it.

    ![Chaos step icon](../../../.gitbook/assets/pipeline-chaos-step-icon.png)
6. In the Configure Chaos Experiment screen, enter a **Name** for this step.
7.  Select **Select Chaos Experiment** to see the chaos experiments you can add to this stage.

    ![Select an existing experiment screen grid of experiments and one experiment selected.](../../../.gitbook/assets/pipeline-select-experiment.png)

    When you select an experiment, the experiment's last resilience score, a preview of the experiment, and its chaos faults, are displayed.
8.  (Optional) On this screen you can:

    * Select **New Experiment** to create a new experiment in Chaos Studio.
    * Select **Edit in Chaos Studio** to edit a selected experiment.

    Selecting these options takes you to Chaos Studio without saving your work.
9. Select the experiment you want to run in this step, and then select **Add to Pipeline**.
10. Back in **Configure Chaos Experiment**, enter the **Expected Resilience Score** for this experiment.

    If the resilience score is not met, this chaos step fails and the [stage failure strategy](https://developer.harness.io/harness-platform/use-harness-platform/pipelines/failure-handling/define-a-failure-strategy-on-stages-and-steps) is initiated.

    For more information, go to Analyze chaos experiments.
11. (Optional) Expand **Optional Configuration**, and enter an assertion (you can enter a fixed value, an expression, or a runtime input).
12. (Optional) Select the **Advanced** tab to configure more settings.

    For more information on these settings, go to [Harness pipelines](https://developer.harness.io/harness-platform/use-harness-platform/pipelines).
13. Select **Apply Changes** to save this step in the pipeline, and then select **Save** to save changes to the pipeline.

**Use runtime inputs in chaos experiments**

When your chaos experiment has runtime inputs configured, those inputs appear in the **Experiment Inputs** section when you add the chaos step to your pipeline. You can then set each input as a pipeline runtime input, so you provide values when the pipeline runs.

Use runtime inputs to:

* Reuse the same chaos experiment across multiple pipelines with different configurations.
* Provide experiment parameters at pipeline execution time.
* Customize chaos validation based on the deployment context.

To use runtime inputs with a chaos experiment:

1. Configure the experiment with runtime inputs. When you create or edit the experiment, select **Runtime Input** instead of **Fixed value** for fault parameters. Go to [Templates](../../../chaos-testing/templates/) to review runtime variable support.
2. Add the chaos step to your pipeline. When you select the experiment, the **Experiment Inputs** section displays all runtime inputs configured in the experiment.
3. Configure each input. Select **Runtime input** to be prompted for a value when the pipeline runs, or select **Fixed value** to provide a static value for this pipeline.

When you run the pipeline, Harness prompts you for all runtime input values before the chaos experiment runs.

**Use templates in chaos steps**

You can add reusable, pre-configured templates to your CD pipeline as chaos steps. This promotes standardization and reduces configuration time across pipelines. The chaos step supports these step-level template types:

* **Experiment templates:** Run a complete chaos experiment from a template.
* **ChaosProbe templates:** Run a probe from a template.
* **ChaosFault templates:** Run a fault from a template.
* **ChaosAction templates:** Run an action from a template.

To use a template as a chaos step:

1. In your pipeline's **Execution** tab, select the **+** icon to add a step.
2.  Select **Use template**.

    ![Use template](../../../.gitbook/assets/use-template.png)
3. In the **Templates** modal, browse the available step and step group templates. Filter by **Scope** (All, Project, Organization, Account), search for a template, and view its version, scope, and inputs.
4.  Select the template you want to use, then select **Use Template**.

    ![Use template](../../../.gitbook/assets/use-template-two.png)
5.  In the **Configure Chaos Experiment** screen, configure the template. For an experiment template, you see:

    * **Template Inputs:** All inputs defined in the template.
    * **Select Chaos Infrastructure:** The infrastructure that runs the experiment.
    * **Expected Resilience Score:** The passing criteria.
    * **Experiment Inputs:** Runtime inputs configured in the experiment, if any.

    Provide a **Fixed value**, a **Runtime input**, or an **Expression** for each input.

    ![Use template](../../../.gitbook/assets/use-template-three.png)
6. Select **Apply Changes** to add the chaos step to your pipeline.

When you select a **ChaosProbe**, **ChaosFault**, or **ChaosAction** template, the configure screen shows that template's own inputs instead of the experiment settings. Provide fixed values, runtime inputs, or expressions for each input the same way, then select **Apply Changes**.

Templates give you:

* **Consistency:** Standardized, tested chaos steps across all pipelines.
* **Efficiency:** Less time configuring chaos steps.
* **Reusability:** Shared configurations across teams and projects.
* **Flexibility:** Combine template inputs with experiment runtime inputs.

Go to [Templates](../../../chaos-testing/templates/) to create and manage chaos experiment, probe, fault, and action templates.

### Step 3: Choose a failure strategy <a href="#step-3-choose-a-failure-strategy" id="step-3-choose-a-failure-strategy"></a>

[A failure strategy](https://developer.harness.io/harness-platform/use-harness-platform/pipelines/failure-handling/define-a-failure-strategy-on-stages-and-steps) specifies the actions to take if pipeline stages or steps fail due to different conditions. You can configure a failure strategy against each chaos step (experiment) in the pipeline, or through a shell script step at the end of the execution of all chaos steps. Below are examples showing these two options.

**Example 1: Failure strategy for one chaos step**

*   In this example, the failure strategy is **Rollback** for **All Errors** on a specific chaos step. You can also choose different types of errors and different strategies to use for each.

    ![Failure strategy for a chaos step](../../../.gitbook/assets/fail-strategy-one-exp.png)

**Example 2: Conditional failure step at the end of several chaos steps**

*   In this example, the failure strategy uses a **Shell Script** step after all the chaos steps in the CD pipeline. This step conditionally applies a shell script that specifies actions to take in case of failure. You can configure this step to add the script, and specify the conditions under which it is executed.

    ![Conditional failure Shell Script step shown in a CD pipeline after many chaos steps](../../../.gitbook/assets/fail-strategy-many-exp.png)

## What happens when the CD pipeline runs with a chaos step <a href="#what-happens-when-the-cd-pipeline-runs-with-a-chaos-step" id="what-happens-when-the-cd-pipeline-runs-with-a-chaos-step"></a>

When the CD pipeline is triggered:

* The chaos step you added to the pipeline triggers the experiment to run on the target application.
* The Chaos Experiments page (**Chaos > Chaos Experiments**) records the experiment run as part of a pipeline, and you can select the experiment to view its execution.
* In the CD pipeline, if the chaos step (the experiment) fails, you can select the failed step to see the log, which includes the resilience score obtained and how many chaos probes passed or failed.
  * You can select **View Detailed Execution** to go to the experiment's execution page in CE.
*   Based on the experiment's success or failure, you can decide whether to continue with the deployment. You can automate this by defining a [failure strategy](harness-cd.md#step-3-choose-a-failure-strategy) in your pipeline.

    For more information, go to [Define a failure strategy on stages and steps](https://developer.harness.io/harness-platform/use-harness-platform/pipelines/failure-handling/define-a-failure-strategy-on-stages-and-steps).

{% @harness-feedback/feedback %}
