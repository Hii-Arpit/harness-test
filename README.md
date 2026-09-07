# Coexist with Karpenter

**Co-exist with Karpenter** lets you run Cluster Orchestrator alongside your existing AWS Karpenter installation instead of replacing it. You keep Karpenter in charge of node provisioning and scaling, and you add only the Cluster Orchestrator capabilities you want, such as telemetry, workload distribution, and cluster schedules. Installation requires no additional AWS permissions, and you can remove the add-ons at any time with no impact on your Karpenter setup.

***

### What you will learn from this topic

* **How it works:** How Cluster Orchestrator runs next to Karpenter and keeps your Karpenter in charge of scaling nodes.
* **What you get:** Which Cluster Orchestrator features work as usual, which ones are limited, and which ones you cannot use while Karpenter is in charge.
* **How to turn it on:** How to enable Co-exist with Karpenter with a single flag, without any extra AWS permissions.
* **When you are ready for more:** How to move to full Cluster Orchestrator and let Harness handle scaling.

***

### Before you begin

Make sure you have the following before you enable **Co-exist with Karpenter**:

* **A working Karpenter setup:** Karpenter is already installed on your EKS cluster and provisioning nodes. **Co-exist with Karpenter** adds to this setup, it does not replace it.
* **A connected cluster:** Your EKS cluster is connected to Harness. Go to [Harness Kubernetes connector](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/connectors/cloud-providers/add-a-kubernetes-cluster-connector) to set one up.
* **The base requirements:** You meet the standard Cluster Orchestrator prerequisites. Go to Get Started to review them.

***

### How Co-exist with Karpenter works

Cluster Orchestrator runs as a set of workloads inside your cluster. Each workload handles one job, such as scaling nodes, distributing pods, or collecting telemetry.

Cluster Orchestrator supports two types of installation: **Default Installation** and **Co-exist with Karpenter**. They differ in which workloads are installed and in whether Cluster Orchestrator or your existing Karpenter scales your nodes.

| Workload                | Resource name                     | Description                                                            | Default Installation | Co-exist with Karpenter |
| ----------------------- | --------------------------------- | ---------------------------------------------------------------------- | :------------------: | :---------------------: |
| **Operator**            | `cluster-orch-operator`           | The main controller that runs Cluster Orchestrator inside your cluster |           ✅          |            ✅            |
| **Distributor**         | `cluster-orch-distributor`        | Places your workloads across nodes                                     |           ✅          |            ✅            |
| **Telemetry Collector** | `cluster-telemetry-collector`     | Sends cluster cost and usage data to Harness                           |           ✅          |            ✅            |
| **Interrupt Listener**  | `cluster-orch-interrupt-listener` | Watches for spot interruptions                                         |           ✅          |            ✅            |
| **Autoscaler**          | `cluster-orch-autoscaler`         | Provisions and scales your nodes                                       |           ✅          |            ❌            |

#### Default Installation

The **Default Installation** installs all five workloads. The **Autoscaler** provisions and scales your nodes, so you scale your existing Karpenter down and **Cluster Orchestrator** manages the cluster on its own. Go to the Installation Guide to set up the Default Installation.

#### Co-exist with Karpenter

**Co-exist with Karpenter** installs the same workloads except the **Autoscaler**. Your existing Karpenter keeps provisioning and scaling nodes, and the other four workloads run alongside it. Because you keep Karpenter in charge, this installation skips the AWS cloud setup and needs no extra AWS permissions.

Because your existing Karpenter setup provisions and scales the nodes, Cluster Orchestrator does not control every action in the cluster. Cluster Orchestrator features therefore fall into three levels of support:

* **Fully supported:** The feature works the same as in the **Default Installation**.
* **Degraded:** The feature works, but with reduced capability, because Karpenter controls node provisioning.
* **Not supported:** The feature is not available, because it requires Cluster Orchestrator to control node provisioning.

Go to Feature support in Co-exist with Karpenter to review the support level for each feature.

***

### Feature support in Co-exist with Karpenter

The following table shows how each Cluster Orchestrator feature behaves when you run Cluster Orchestrator alongside your existing Karpenter.

| Feature                       | Support level   | Description                                                                                                                                                                  |
| ----------------------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Telemetry collection          | Fully supported | Collects the same cost and usage data as the **Default Installation**                                                                                                        |
| Workload distribution         | Degraded        | Pod-level distribution works, but provisioning-time distribution does not, because Cluster Orchestrator does not control node provisioning                                   |
| Cluster schedules             | Fully supported | Karpenter removes idle nodes as your workloads scale down                                                                                                                    |
| VPA                           | Fully supported | Karpenter provisions nodes sized to fit the pod resource requests that VPA sets                                                                                              |
| NodePools and NodeClass UI    | Degraded        | The UI is likely empty because the NodePool sync runs in the autoscaler, which is not installed in this mode                                                                 |
| Bin packing                   | Degraded        | Karpenter's built-in consolidation is not the same as Cluster Orchestrator's bin packing feature; the Harness pod evictor requires the autoscaler for NodePool configuration |
| Spot orchestration            | Not supported   | The interrupt listener, cordon, provision, and drain logic all run inside the autoscaler pod, which is not installed in this mode                                            |
| Spot-to-spot consolidation    | Not supported   | Only applied through the Harness autoscaler's Karpenter integration, which is not installed in this mode                                                                     |
| Commitment integration        | Not supported   | Not available, because Karpenter decides when to use committed capacity, not Cluster Orchestrator                                                                            |
| Fallback and reverse fallback | Not supported   | Not available, because Cluster Orchestrator does not control which instances are provisioned                                                                                 |
| Replacement schedules         | Not supported   | Not available, because Cluster Orchestrator does not replace nodes in this installation                                                                                      |
| Distribution strategy         | Not supported   | Not available, because Cluster Orchestrator does not control how instances are selected                                                                                      |

***

### Enable Co-exist with Karpenter

Enable Co-exist with Karpenter by running the Cluster Orchestrator enablement script with the autoscaler disabled. This installs the Cluster Orchestrator add-ons without the autoscaler and without any AWS cloud setup, so no additional AWS permissions are required.

{% hint style="info" %}
The `AUTOSCALER_ENABLED` flag is only available on the AWS enablement script.
{% endhint %}

1. Log in to [Harness](https://app.harness.io) and go to **Cloud Costs** → **Cluster Orchestrator**.
2. Select your cluster, then open the configuration screen.
3. Copy the generated enablement script.
4.  Add `AUTOSCALER_ENABLED=false` manually at the start of the copied command, then run it.

    The Harness UI does not add this flag for you. Copy the generated command and add `AUTOSCALER_ENABLED=false` at the start before running it.

    ```bash
    AUTOSCALER_ENABLED=false <generated enablement script>
    ```

    With `AUTOSCALER_ENABLED=false`, the script skips the autoscaler workload and the AWS cloud setup, and installs only the Kubernetes workloads that run alongside Karpenter.

    For example, the generated command looks similar to the following, with `AUTOSCALER_ENABLED=false` added at the start:

    ```bash
    AUTOSCALER_ENABLED=false \
      CCM_K8S_CONNECTOR_ID=my_eks_connector \
      TOKEN=<harness_api_token> \
      CLUSTER_NAME="my-eks-cluster" \
      REGION=us-east-2 \
      VPA_ENABLED=false \
      /bin/bash -c "$(curl -H 'x-api-key: <harness_api_token>' -fsSL 'https://app.harness.io/lw/api/accounts/<account_id>/clusters/orchestrator/onboard?accountIdentifier=<account_id>')"
    ```

    <div data-gb-custom-block data-tag="hint" data-style="info" class="hint hint-info"><p>VPA is disabled by default. To use VPA in Co-exist with Karpenter, add <code>VPA_ENABLED=true</code> to the command.</p></div>
5.  Verify the installation. In Co-exist with Karpenter, the four Cluster Orchestrator workloads run and the `cluster-orch-autoscaler` deployment is absent:

    ```bash
    kubectl get deploy,daemonset -n kube-system | grep "cluster-orch\|cluster-telemetry"
    ```

    Confirm the `cluster-orch-operator`, `cluster-orch-distributor`, and `cluster-telemetry-collector` deployments and the `cluster-orch-interrupt-listener` DaemonSet are present, and that `cluster-orch-autoscaler` is not.

***

### Migrate to full Cluster Orchestrator

When you are ready to let Harness manage node provisioning, migrate from Co-exist with Karpenter to the full installation:

1. Scale down your Karpenter deployment.
2. Re-run the enablement script with the autoscaler enabled.

After migration, the degraded and unsupported features become fully available, and Cluster Orchestrator manages all node scaling. Go to the Installation Guide to review the full installation.

***

### Troubleshooting

<details>

<summary>The NodePools and NodeClass UI is empty after installing Co-exist with Karpenter.</summary>

This is expected. NodePool sync runs inside the autoscaler, which is not installed in Co-exist with Karpenter. The UI populates only after you migrate to the full installation.

</details>

<details>

<summary>The interrupt listener is reporting errors in Co-exist with Karpenter.</summary>

This is expected. The interrupt listener is installed but the spot handling logic (cordon, provision, drain) runs inside the autoscaler pod, which is not installed in this mode. Interrupt listener errors in co-exist mode do not affect your cluster. Migrate to the full installation to enable full spot orchestration.

</details>

<details>

<summary>A feature such as bin packing or workload distribution is enabled in the Harness UI but has no effect.</summary>

Several Cluster Orchestrator features depend on the autoscaler, which is not installed in Co-exist with Karpenter. Enabling these features in the UI has no effect in this mode. Go to Feature support in Co-exist with Karpenter to review which features are available, and migrate to the full installation to use autoscaler-dependent features.

</details>

<details>

<summary>A Cluster Orchestrator feature such as commitment integration or reverse fallback has no effect in Co-exist with Karpenter.</summary>

These features are not supported in Co-exist with Karpenter because they require Cluster Orchestrator to control node provisioning. Migrate to the full installation to use them.

</details>

***

### Next steps

* Go to Cluster Orchestrator vs Karpenter to compare the two approaches.
* Go to Features of Cluster Orchestrator to configure the add-ons.
