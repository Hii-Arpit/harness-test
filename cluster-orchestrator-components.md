---
description: >-
  Compare Harness Cluster Orchestrator with AWS Karpenter and discover unique
  advantages
---


# Cluster Orchestrator Components

Cluster Orchestrator deploys several Kubernetes resources to manage and optimize your cluster infrastructure. This document provides a comprehensive overview of all components installed during deployment.

Cluster Orchestrator installation creates a comprehensive set of infrastructure components across both AWS and Kubernetes environments:

### Roles <a href="#roles" id="roles"></a>

#### AWS <a href="#aws" id="aws"></a>

| Resource              | Name                              | Description                                                                       |
| --------------------- | --------------------------------- | --------------------------------------------------------------------------------- |
| **IAM Role**          | `harness-ccm-<cluster>-node`      | IAM role for EKS worker nodes with CCM-required policies                          |
| **Access Entry**      | -                                 | Newer EKS "Access Entries" system to bind IAM role into Kubernetes (system:nodes) |
| **Instance Profile**  | `harness-ccm-<cluster>-inst-prof` | Allows EC2 nodes to assume the node IAM role                                      |
| **OIDC Provider**     | -                                 | Enables IRSA (IAM Roles for Service Accounts) for secure AWS IAM usage            |
| **IAM Policy Update** | -                                 | Adds extra describe/EC2 permissions required for CCM                              |
| **Resource Tagging**  | `harness.io/<cluster>`            | Marks infrastructure (SGs, Subnets, AMIs) for CCM resource discovery              |

#### Harness <a href="#harness" id="harness"></a>

| Resource                 | Name | Description                                                               |
| ------------------------ | ---- | ------------------------------------------------------------------------- |
| **Service Account**      | -    | Created via API to represent the cluster orchestrator                     |
| **API Key + Token**      | -    | Issued and stored in K8s as a secret for cluster-to-Harness communication |
| **Cluster Registration** | -    | Registers your EKS cluster with Harness CACM                              |

### Components <a href="#components" id="components"></a>

#### Core resources <a href="#core-resources" id="core-resources"></a>

| Resource Type | Name              | Description                                           |
| ------------- | ----------------- | ----------------------------------------------------- |
| **Namespace** | `kube-system`     | Default namespace where CCM orchestrator is installed |
| **Secret**    | `harness-api-key` | Stores Harness API token for cluster components       |

#### Cluster Orchestrator components <a href="#cluster-orchestrator-components" id="cluster-orchestrator-components"></a>

| Component Type | Name                                        | Description                                                                            |
| -------------- | ------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Deployment** | `cluster-orch-operator` (harness-ccm)       | Main controller managing CCM orchestration inside cluster. Limits: CPU: 1, Memory: 1Gi |
| **Deployment** | `cluster-orch-distributor` (kube-system)    | Manages workload distribution across nodes. Limits: CPU: 500m, Memory: 512Mi |
| **Deployment** | `cluster-telemetry-collector` (kube-system) | Collects cluster cost/usage data and sends it to Harness. Limits: CPU: 1, Memory: 1Gi  |
| **Deployment** | `cluster-orch-autoscaler` (kube-system)     | Provisions and scales nodes by encapsulating Karpenter functionality. Skipped in [Co-exist with Karpenter](co-exist-with-karpenter.md). Limits: CPU: 1, Memory: 1Gi |
| **Service**    | `cluster-orch-operator`                     | Cluster Orchestrator operator service                                                  |
| **DaemonSet**  | `cluster-orch-interrupt-listener`           | Listens for spot preemption events and reports to Harness                              |

#### Service accounts & RBAC <a href="#service-accounts-and-rbac" id="service-accounts-and-rbac"></a>

Each of these components gets a ServiceAccount, ClusterRole, and ClusterRoleBinding:

| Service Account                         | Purpose                                              |
| --------------------------------------- | ---------------------------------------------------- |
| `ccm-cluster-orchestrator-operator`     | Permissions for the orchestrator operator deployment |
| `ccm-cluster-orchestrator-distributor`  | Permissions for the workload distributor deployment  |
| `ccm-cluster-orchestrator-metrics`      | Handles metrics collection for observability         |
| `ccm-cluster-orchestrator-interruption` | Manages pod eviction/node interruption handling      |
| `ccm-cluster-orchestrator-autoscaler`   | Permissions for the autoscaler deployment that provisions and scales nodes |

#### Custom resource definitions (CRDs) <a href="#custom-resource-definitions-crds" id="custom-resource-definitions-crds"></a>

| CRD Name                                   | Purpose                                                                |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| `nodepools.karpenter.sh`                   | Defines node pools for Karpenter provisioning                          |
| `nodeclaims.karpenter.sh`                  | Manages node claims for Karpenter                                      |
| `ec2nodeclasses.karpenter.k8s.aws`         | Defines EC2 node class templates for Karpenter provisioning            |
| `workloaddistributionrules.ccm.harness.io` | Harness CACM custom CRD to control workload placement and distribution |

{% @harness-feedback/feedback %}
