---
description: Introduction to eBPF based Autostopping for Kubernetes.
---


# Kubernetes Autostopping eBPF

### Overview <a href="#overview" id="overview"></a>

Kubernetes Autostopping V2 is a complete re-architecture of the original Autostopping (V1) solution, aimed at reducing operational complexity, improving traffic detection, easy onboarding and enabling more efficient scaling for Kubernetes workloads. In earlier version of AutoStopping, traffic was routed through an Envoy based router for both traffic detection and workload rewiring. In eBPF based AutoStopping, we’ve eliminated the router entirely, introduced Ingress-based rewiring, and built a new eBPF-powered Traffic Detection Agent for high-accuracy traffic monitoring and mapping cluster networking. This change brings multi-workload support in a single rule, multiple traffic entry points per workload, and simpler, more scalable architecture.

### Key changes from V1 to V2 <a href="#key-changes-from-v1-to-v2" id="key-changes-from-v1-to-v2"></a>

#### 1. Removal of proxy layer <a href="#id-1-removal-of-proxy-layer" id="id-1-removal-of-proxy-layer"></a>

**Earlier:**

* All incoming traffic flowed through a proxy.
* Proxy handled both traffic detection and rewiring to workloads.
* Rewiring to workloads was done once at setup; subsequent rewiring occurred only inside the proxy.

**eBPF based AutoStopping:**

* Proxy has been removed entirely.
* Rewiring is now done directly on the Ingress.
* Controller updates Ingress routes dynamically whenever workloads are stopped or started.
* V2 currently supports NGINX Ingress only.

#### 2. New traffic detection agent (eBPF-based) <a href="#id-2-new-traffic-detection-agent-ebpf-based" id="id-2-new-traffic-detection-agent-ebpf-based"></a>

**Why:** Without the router, we needed a new way to detect traffic without being in the direct data path.

**How it works:**

* Runs as a DaemonSet across all nodes in the cluster.
* Uses eBPF to monitor low-level network activity directly from the kernel.
* Detects when workloads receive traffic without introducing latency.

**Requirements:**

* Works only on node-based Kubernetes clusters (VM-based).
* Not supported on serverless or fully managed offerings like AWS Fargate or GCP Autopilot.
* Requires host-level access and elevated permissions in the deployment YAML.

#### 3. Multiple workloads per Autostopping rule <a href="#id-3-multiple-workloads-per-autostopping-rule" id="id-3-multiple-workloads-per-autostopping-rule"></a>

**V1 Limitation:**

* Each Autostopping rule could only manage one workload.
* Example: For an application with 100 workloads, 100 separate rules were required for independent traffic-based scaling.

**V2 Improvement:**

* A single Autostopping rule can now manage multiple workloads that form a complete application.
* Traffic to any workload in the group will keep the entire application running.
* Greatly reduces configuration overhead and simplifies operations.

#### 4. Multiple traffic entry points <a href="#id-4-multiple-traffic-entry-points" id="id-4-multiple-traffic-entry-points"></a>

V2 supports defining multiple ingress routes for the same workload.

**This means:**

* Whether traffic comes via external.example.com or internal.example.com (or any other configured host/path), the application will start automatically.
* Useful for workloads accessed from multiple domains, subdomains, or ingress controllers.

### Technical requirements <a href="#technical-requirements" id="technical-requirements"></a>

| Feature                    | Requirement                                                          |
| -------------------------- | -------------------------------------------------------------------- |
| Cluster Type               | Node-based (VM) Kubernetes cluster                                   |
| Ingress Controller Support | NGINX Ingress (others to be added later)                             |
| Traffic Detection          | eBPF agent in node kernel                                            |
| Permissions                | Host-level access + elevated permissions for Traffic Detection Agent |

### Migration for existing users <a href="#migration-for-existing-users" id="migration-for-existing-users"></a>

Upgrading to Autostopping V2 is designed to be frictionless for existing V1 users.

#### Before you upgrade <a href="#before-you-upgrade" id="before-you-upgrade"></a>

* Ensure the workloads managed by your V1 Autostopping rules are running at the time of upgrade. If workloads are stopped, they will not be considered by the migration process and will need to be recreated manually in V2.
* Confirm your cluster meets V2 requirements (NGINX Ingress, node-based cluster, eBPF support, elevated permissions).

#### Upgrade steps <a href="#upgrade-steps" id="upgrade-steps"></a>

* Install or upgrade to Autostopping Controller v2.0 in your cluster.
* The controller automatically:
  * Detects existing V1 rules.
  * Migrates them to V2 format.
  * Reconfigures Ingress rewiring and traffic detection to use the new architecture.
* No downtime is expected during migration, but incoming requests may be briefly queued if workloads are stopped and need to be restarted.

#### Sample template: <a href="#sample-template" id="sample-template"></a>

<figure><img src="../../.gitbook/assets/ebpf.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

\>

{% @harness-feedback/feedback %}
