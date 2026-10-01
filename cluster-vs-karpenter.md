---
description: >-
  Compare Harness Cluster Orchestrator with AWS Karpenter and discover unique
  advantages
---


# Harness Cluster Orchestrator vs AWS Karpenter

## 1. Spot node orchestration <a href="#id-1-spot-node-orchestration" id="id-1-spot-node-orchestration"></a>

| Feature                         | Karpenter                                                                             | Cluster Orchestrator                                                                                                                                                                                                                                          |
| ------------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Setup Complexity**            | Requires manual SQS Queue setup and maintenance                                       | Works out-of-the-box with zero configuration                                                                                                                                                                                                                  |
| **Interruption Handling**       | Basic SQS-based monitoring                                                            | Sophisticated interruption handling                                                                                                                                                                                                                           |
| **Node Selection**              | Limited strategy options                                                              | Configurable strategies (cost-optimized, least-interrupted)                                                                                                                                                                                                   |
| **Fallback & Reverse Fallback** | Basic fallback: moves pods to On-Demand once Spot is interrupted; no automatic return | Intelligent fallback automatically launches an On-Demand instance when Spot capacity gets unavailable. If market is unavailable, it creates an On Demand instance. And **reverse fallback** seamlessly moves workloads back to Spot when capacity is healthy. |

## 2. Cost visibility and savings analysis <a href="#id-2-cost-visibility-and-savings-analysis" id="id-2-cost-visibility-and-savings-analysis"></a>

| Feature                     | Karpenter                                                   | Cluster Orchestrator                                                             |
| --------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Cost Savings Visibility** | No visibility into cost savings achieved through Spot usage | Real-Time Savings Insights: Track actual cost savings from Spot node utilization |
| **Cluster Cost Insights**   | Lacks insights into cluster cost optimization potential     | Comprehensive Cost Analysis: Perspectives provide deep cluster cost visibility   |

## 3. Intelligent bin packing <a href="#id-3-intelligent-bin-packing" id="id-3-intelligent-bin-packing"></a>

| Feature                   | Karpenter                                                       | Cluster Orchestrator                                                                                  |
| ------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Granular Thresholds**   | Fixed consolidation logic; cannot tune under-utilisation levels | Customisable CPU & Memory thresholds that drive extra bin-packing on top of Karpenter’s consolidation |
| **Pod Evictions**         | Basic eviction                                                  | Evicts pods intelligently while honoring **Pod Disruption Budgets**                                   |
| **Resulting Utilisation** | Moderate                                                        | Maximised node utilisation and lower waste                                                            |

## 4. Dynamic spot/on-demand split configuration <a href="#id-4-dynamic-spoton-demand-split-configuration" id="id-4-dynamic-spoton-demand-split-configuration"></a>

| Capability                              | Karpenter     | Cluster Orchestrator                                                        |
| --------------------------------------- | ------------- | --------------------------------------------------------------------------- |
| **Percentage-based Spot/On-Demand mix** | Not available | Specify exact Spot vs On-Demand percentages via `WorkloadDistributionRules` |
| **Base capacity safeguards**            | Not available | Ensure minimum On-Demand replicas for critical workloads                    |

## 5. Commitment utilization guarantee <a href="#id-5-commitment-utilization-guarantee" id="id-5-commitment-utilization-guarantee"></a>

| Feature                           | Karpenter                         | Cluster Orchestrator                                                                                                        |
| --------------------------------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **RI / Savings Plan awareness**   | Limited; manual tracking required | Fully integrated with [Harness Commitment Orchestrator](https://developer.harness.io/docs/category/commitment-orchestrator) |
| **Automatic node type selection** | Not supported                     | Launches nodes that consume idle commitments first                                                                          |
| **Over-provisioning risk**        | High if data outdated             | Minimized by commitment-aware scheduling                                                                                    |

## 6. Replacement windows <a href="#id-6-replacement-windows" id="id-6-replacement-windows"></a>

| Capability                                 | Karpenter                               | Cluster Orchestrator                                                                                                                                                                                 |
| ------------------------------------------ | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Time-window control for disruptive ops** | Available (NodePool Disruption Budgets) | **Replacement Windows** let you pre-schedule disruptive operations such as Bin Packing, Harness pod eviction, consolidation, and reverse fallback so that they run outside critical business periods |

{% @harness-feedback/feedback %}
