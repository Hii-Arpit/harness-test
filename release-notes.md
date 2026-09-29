---
description: Release notes for Cluster Orchestrator
---


# Release Notes

{% hint style="info" %}
Currently, this early access feature is behind a feature flag. Contact [Harness Support](mailto:support@harness.io) to enable the feature.
{% endhint %}

## Compatibility Matrix <a href="#compatibility-matrix" id="compatibility-matrix"></a>

| Cluster Orchestrator Version | Kubernetes | Karpenter |
| ---------------------------- | ---------- | --------- |
| Till `0.6.0`                 | 1.32       | 1.2.4     |
| `0.7.0`                      | 1.33       | 1.7.3     |
| `beta-0.7.0`                 | 1.34       | 1.8.0     |
| `0.9.0`                      | 1.34       | 1.8.2     |

## Operator <a href="#operator" id="operator"></a>

Link: https://hub.docker.com/r/harness/cluster-orchestrator/tags

| Version | Release Date | Change Log |
| ------- | ------------ | ---------- |
| **LATEST** `0.9.0` | April 28, 2026 | <ul><li>Upgraded Karpenter dependency to v1.8.2. Operator now emits events to the platform on schedule, Start and End.</li><li>Thread-safe logging in Reconcilers; CCM Taint-related noise suppressed in the operator.</li><li>Fixed pod scheduled status check to use node assignment instead of pod phase.</li><li>Removed unnecessary/noisy error log lines from the operator.</li><li>Stabilised Cluster Schedules (Beta)</li></ul> |
| `0.8.2` | Mar 6, 2026 | Complete redesign of bin-packing to work seamlessly with Karpenter consolidation, delivering better cost savings with zero conflicts:<ul><li>Uses pod resource requests (matching Karpenter's logic) to eliminate evict-and-reprovision churn.</li><li>Only consolidates Karpenter-managed nodes that have been stable for ConsolidateAfter duration.</li><li>Cordons nodes immediately after eviction to prevent reschedule races.</li><li>Respects karpenter.sh/do-not-disrupt annotation at pod and node levels.</li><li>Skips nodes already being disrupted by Karpenter.</li><li>Protects system-critical pods, single replicas, and PDBs.</li><li>Configurable eviction caps (default: 20 pods) to prevent storms.</li><li>Rich platform events with CPU/Memory utilization data.</li><li>Non-blocking async execution.</li><li>Azure Karpenter Support (Preview).</li><li>Integrated Azure Karpenter provider for AKS support.</li><li>Multi-cloud providerID parsing (AWS, Azure, GCP).</li><li>Configurable Azure Karpenter version support.</li></ul> |
| `0.8.1` | Feb 23, 2026 | <ul><li>Upgraded to VPA 1.5.1 with support for in-place pod resizing (eliminates pod restarts during resource adjustments).</li><li>Added real-time scaling event tracking for VPA operations.</li><li>Enhanced security with AWS IMDSv2 support and Rapidfort hardened container images.</li><li>Performance improvements with Go 1.25 runtime upgrade.</li></ul> |
| `0.8.0` | Feb 3, 2026 | <ul><li>Introducing a new distributor component (currently under a **FF DISTRIBUTIONV2\_ENABLED**).</li><li>This component works with a checkpoint to ensure the distribution of spot and on-demand is as per the distribution rules.</li><li>Faster distribution: faster placement and reconciliation for eventual consistency of the split.</li><li>Bug fix: Editing the config, disabling the harness managed nodepools.</li><li>Suppressed client-go logs (available under log level TRACE).</li><li>Adding Default Consolidation on Harness created default nodepools.</li><li>Enabling Nodepools and Nodeclasses sync by default.</li></ul> |
| `0.7.0` | Dec 12, 2025 | <ul><li>Cluster Orchestrator supports Karpenter 1.7.3 features. Users must [re-run the enablement script or Terraform template](enablement-methods/setting-up-co-helm.md#step-1-set-up-required-infrastructure-with-terraform) and perform a Helm upgrade to add new permissions for Cluster Orchestrator related to Karpenter 1.7.3.</li><li>Supports Kubernetes (EKS) version 1.33; previously supported only 1.32.</li><li>New major Karpenter features (from 1.2.4 to 1.7.3) for non-Karpenter customers: Support for ODCRs ([on-demand capacity reservations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-capacity-reservations.html)), SSM Parameter support for AMI selection, Improved [IAM instance profile management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) and Bug fixes and performance improvements.</li><li>Auto-Discovery Support for Instance Profile and Node Role ARN Environment Variables. Environment Variables with Auto-Discovery Support: `AWS_DEFAULT_INSTANCE_PROFILE`, `AWS_NODE_ROLE_ARN`. Previously, these environment variables were required to be explicitly defined in the deployment configuration. Now, the Cluster Orchestrator automatically discovers these values on startup.</li></ul> |
| `0.6.0` | Nov 26, 2025 | <ul><li>Support for Vertical Pod AutoScaling (VPA automatically adjusts CPU and memory resource requests for pods based on their usage patterns). This is behind a feature flag, please reach out to [support@harness.io](mailto:support@harness.io) to enable it.</li><li>New CRD and deployments added.</li><li>Support for [Disabling and Deleting Cluster Orchestrator](https://developer.harness.io/cloud-cost-management/cost-optimization/cluster-orchestrator-for-aws-eks-clusters/enablement-methods/delete).</li></ul> |
| `0.5.2` | Oct 3, 2025 | <ul><li>Support for Disabling Cluster Orchestrator via Config CRD.</li><li>Support for configuring Cluster Level Nodepool limits via Config CRD.</li><li>Improved validation of the Replacement window.</li><li>Handling lease locks gracefully.</li><li>Handling finalizer errors gracefully.</li></ul> |
| `0.5.1` | Sep 3, 2025 | <ul><li>Support for **Robust Distribution mode** in Bin Packing. **Robust Distribution Mode** is an enhancement to the default Cluster Orchestrator scheduling flow which gets automatically enabled when bin packing is enabled. Using the Kubernetes Descheduler's policy `RemovePodsViolatingNodeTaints`, it automatically evicts pods from incompatible nodes and reschedules them to nodes that match your configured distribution.</li></ul> |
| `beta-0.5.0` | Aug 6, 2025 | <ul><li>Support for Kubernetes version 1.32 (Karpenter version 1.2).</li><li>New CRD `ClusterOrchestratorConfig` enabling Kubernetes-native configuration of Cluster Orchestrator directly within the cluster — allows users to declaratively define nodepool settings, spot/on-demand distribution, bin-packing parameters, and workload scheduling policies as standard Kubernetes resources.</li><li>Fixed the bug where 100% On demand configuration was creating spot nodes under heavy load.</li></ul> |
| `beta-0.4.3` | July 1, 2025 | <ul><li>Suppressing error logs.</li><li>Support for disabling default nodepools via an ENV variable `DISABLE_DEFAULT_NODEPOOLS`.</li><li>Handling Null Object scenarios in Cluster Orchestrator reconcilers.</li><li>Handling nodepools with maximum nodepool weight 100.</li><li>Working well with other operators (like Kyverno) which adds tolerations on pods.</li></ul> |
| `beta-0.4.2` | June 17, 2025 | <ul><li>Sped up the distribution of workloads, by iterating the changed ReplicaSet instead of the whole namespace.</li><li>Adding default toleration for pods in protected namespaces.</li><li>Handling Distribution of Job Pods separately, irrespective of namespace triggers.</li></ul> |
| `beta-0.4.1` | June 1, 2025 | <ul><li>Consolidated to a unified ReplicaSet reconciler architecture that handles both Deployments and StatefulSets.</li><li>Handling the scheduling of Stand Alone Pods with default toleration.</li><li>[Replacement windows](https://developer.harness.io/release-notes/cloud-cost-management#-new-feature-replacement-schedules) for Bin Packing, Consolidation, Reverse Fallback.</li></ul> |
| `beta-0.4.0` | May 6, 2025 | <ul><li>Workload Distribution Rule at namespace level.</li><li>Integration with commitment orchestrator.</li><li>Event logs for Cluster Orchestrator.</li><li>Node replacement Framework.</li></ul> |

## Helm Chart Releases <a href="#helm-chart-releases" id="helm-chart-releases"></a>

Link: https://app.harness.io/ng/account/6NTMT--yR7ORXKPqwLDioA/module/code/repos/Helm

| Version | Release Date | Changes |
| ------- | ------------ | ------- |
| **LATEST** `0.5.0` | April 28, 2026 | <ul><li>Support for 0.9.0.</li><li>Karpenter dependency 1.8.2.</li></ul> |
| `0.4.2` | Mar 6, 2026 | <ul><li>Support for 0.8.2.</li><li>Updated the Config CRD to support Bin Packing `max_evictions`.</li><li>Updated permissions for Node and Pod metrics calculation.</li></ul> |
| `0.4.1` | Feb 23, 2026 | <ul><li>Support for Cluster Orchestrator operator version 0.8.1.</li><li>Updated the HVPA (Harness Vertical Pod Autoscaler) CRD to support in-place pod resizing, enabling resource adjustments without pod restarts.</li><li>Added RBAC permissions required for in-place pod resizing operations.</li></ul> |
| `0.4.0` | Feb 3, 2026 | <ul><li>Support for 0.8.0 build.</li><li>New CRD `WorkloadDistributionRuleCheckpoint`.</li><li>New Deployment Distributor and its permissions.</li><li>New component can be enabled via the value `clusterOrchestrator.distributor.enabled`.</li><li>Adding `priorityClass` for Cluster Orchestrator components.</li></ul> |
| `0.3.0` | Dec 12, 2025 | <ul><li>Ability to support Karpenter 1.7.3 features in Cluster Orchestrator.</li></ul> |
| `0.2.0` | Nov 26, 2025 | <ul><li>Ability to install Harness VPA components and CRD (if feature flag is enabled).</li></ul> |
| `0.1.4` | Oct 3, 2025 | <ul><li>Supporting 0.5.2 build.</li><li>Introducing `cluster_preferences` with `nodepool_limits` in the config CRD.</li><li>Introducing `disabled` field in the config CRD to toggle cluster orchestrator state.</li><li>Validation and permission fixes.</li></ul> |
| `0.1.3` | Sep 16, 2025 | <ul><li>Conditional Installation of Karpenter CRDs: controlled by the variable `clusterOrchestrator.karpenter.installCRD`.</li><li>Conditional management of namespace: If the namespace is in the protected namespaces list, this helm chart will not manage the namespace. Controlled by variable `clusterOrchestrator.operator.protectedNamespaces`.</li><li>Conditional installation of CRDs with `clusterOrchestrator.installCRDs` variable.</li><li>Adding SecretRefs to manage sensitive fields.</li><li>Readme and QuickStart files added.</li></ul> |
| `0.1.2` | Sep 3, 2025 | <ul><li>Uses build version `0.5.1`.</li></ul> |
| `0.1.1` | Aug 7, 2025 | <ul><li>Added validation for Node Expiry field.</li><li>Defaulting Node Expiry to 720h.</li></ul> |
| `0.1.0` | Aug 6, 2025 | <ul><li>Uses build version beta-0.5.0.</li><li>Added new CRD `ClusterOrchestratorConfig` and related permissions.</li></ul> |
| `0.0.13` | Jul 1, 2025 | <ul><li>Setting the default value for `DISABLE_DEFAULT_NODEPOOLS` as true.</li></ul> |
| `0.0.12` | Jul 1, 2025 | <ul><li>Uses build version beta-0.4.3.</li><li>Ability to disable default nodepools via an ENV variable `DISABLE_DEFAULT_NODEPOOLS`.</li></ul> |
| `0.0.11` | Jun 17, 2025 | <ul><li>Ability to configure nodeSelector and Tolerations on Cluster Orchestrator Deployments.</li><li>Uses build version beta-0.4.2.</li></ul> |
| `0.0.10` | Jun 10, 2025 | <ul><li>Fixed the liveliness and readiness check probe ports.</li><li>Uses build version beta-0.4.1.</li></ul> |
| `0.0.9` | May 19, 2025 | <ul><li>Fixed memory configuration for Cluster Orchestrator Deployments.</li></ul> |

## Cluster Orchestrator Beta Release <a href="#cluster-orchestrator-beta-release" id="cluster-orchestrator-beta-release"></a>

| Version | Release Date | Changes |
| ------- | ------------ | ------- |
| **LATEST** `0.7.0-beta.1` | Jan 14, 2025 | <ul><li>Supports Kubernetes 1.34 version (Current stable release supports till 1.33 only).</li><li>Supports Karpenter 1.8 Version and its newer features.</li><li>Support Static Capacity Mode of NodePools to maintain a fixed number of nodes regardless of pod demand.</li><li>To migrate from current stable version to this beta version, users have to run onboarding script/helm install command with FF enabled.</li><li>To install Cluster Orchestrator Beta, set `CLUSTER_ORCH_CHANNEL=beta` as an environment variable while running onboarding script.</li><li>To install Cluster Orchestrator Beta, pass `--version=0.3.0-beta.1` in the Helm command.</li></ul> |

{% @harness-feedback/feedback %}
