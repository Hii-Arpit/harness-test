---
description: Comprehensive library of pre-built chaos faults for testing system resilience
---


# Chaos Faults

Chaos faults are the failures injected into the chaos infrastructure as part of a chaos experiment. Every fault is associated with a target resource, and you can customize the fault using the fault tunables, which you can define as part of the Chaos Experiment CR and Chaos Engine CR.

The fault execution is triggered when the chaos engine resource is created. Typically, the chaos engine is embedded within the **steps** of a chaos fault. However, you can also create the chaos engine manually, and the chaos operator reconciles this resource and triggers the fault execution.

You can customize a fault execution by changing the tunables (or parameters). Some tunables are common across all the faults (for example, **chaos duration**), and every fault has its own set of tunables: default and mandatory ones. You can update the default tunables when required and always provide values for mandatory tunables (as the name suggests).

## Fault Status <a href="#fault-status" id="fault-status"></a>

Fault status indicates the current status of the fault executed as a part of the chaos experiment. A fault can have 0, 1, or more associated [probes](probes/README.md). Other steps in a chaos experiment include resource creation and cleanup.

In a chaos experiment, a fault can be in one of six different states. It transitions from **running**, **stopped** or **skipped** to **completed**, **completed with error** or **error** state.

* **Running**: The fault is currently being executed.
* **Stopped**: The fault stopped after running for some time.
* **Skipped**: The fault skipped, that is, the fault is not executed.
* **Completed**: The fault completes execution without any **failed** or **N/A** probe statuses.
* **Completed with Error**: When the fault completes execution with at least one **failed** probe status but no **N/A** probe status, it is considered to be **completed with error**.
* **Error**: When the fault completes execution with at least one **N/A** probe status, it is considered to be **error** because you can't determine if the probe status was **passed** or **failed**. A fault is considered to be in an **error** state when it has 0 probes because there are no health checks to validate the sanity of the chaos experiment.

## Fault Categories <a href="#fault-categories" id="fault-categories"></a>

Harness Chaos Engineering provides a comprehensive library of pre-built chaos faults organized by target infrastructure and platform. Below are tables with links to individual fault documentation for easy navigation.

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><img src="../.gitbook/assets/image (2).png" alt="AWS" data-size="line"></td><td align="center"><strong>AWS</strong><br>Chaos faults for AWS</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/aws/">Chaos faults for AWS</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (3).png" alt="Azure" data-size="line"></td><td align="center"><strong>Azure</strong><br>Chaos faults for Azure</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/azure/">Chaos faults for Azure</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (4).png" alt="Cloud Foundry" data-size="line"></td><td align="center"><strong>Cloud Foundry</strong><br>Chaos faults for Cloud Foundry</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/cloud-foundry/">Cloud Foundry</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (5).png" alt="GCP" data-size="line"></td><td align="center">Chaos faults for GCP</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/gcp/">Chaos faults for GCP</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (6).png" alt="Kube-resilience" data-size="line"></td><td align="center">Chaos faults for Kube-resilience</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/kube-resilience/">Chaos faults for Kube-resilience</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (1).png" alt="Kubernetes" data-size="line"></td><td align="center">Chaos faults for Kubernetes</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/kubernetes/">Chaos Faults for Kubernetes</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (7).png" alt="Linux" data-size="line"></td><td align="center">Chaos faults for Linux</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/linux/">Chaos Faults for Linux</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (8).png" alt="Load" data-size="line"></td><td align="center">Chaos faults for Load</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/load/">Chaos faults for load generation</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (9).png" alt="SSH" data-size="line"></td><td align="center">Chaos faults for SSH</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/ssh/">Chaos faults for SSH</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image (10).png" alt="VMware" data-size="line"></td><td align="center">Chaos faults for VMware</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/vmware/">Chaos faults for VMware</a></td></tr><tr><td align="center"><img src="../.gitbook/assets/image.png" alt="Windows" data-size="line"></td><td align="center">Chaos faults for Windows</td><td><a href="https://developer.harness.io/docs/chaos-engineering/faults/chaos-faults/windows/">Chaos faults for Windows</a></td></tr></tbody></table>

## Common Fault Tunables <a href="#common-fault-tunables" id="common-fault-tunables"></a>

Fault tunables common to all the faults are provided at `.spec.experiment[*].spec.components.env` in the chaosengine.

### Duration of the chaos <a href="#duration-of-the-chaos" id="duration-of-the-chaos"></a>

Total duration of the chaos injection (in seconds). Tune it by using the `TOTAL_CHAOS_DURATION` environment variable.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# defines total time duration of the chaos 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        # time duration for the chaos execution
        - name: TOTAL_CHAOS_DURATION
          VALUE: '60'
```

### Chaos interval <a href="#chaos-interval" id="chaos-interval"></a>

The delay between each chaos iteration. Multiple iterations of chaos are tuned by setting the `CHAOS_INTERVAL` environment variable.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# defines delay between each successive iteration of the chaos 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        # delay between each iteration of chaos
        - name: CHAOS_INTERVAL
          value: '15'
        # time duration for the chaos execution
        - name: TOTAL_CHAOS_DURATION
          VALUE: '60'
```

### Ramp time <a href="#ramp-time" id="ramp-time"></a>

Period to wait before and after injecting chaos. It is in units of seconds. Tune it by using the `RAMP_TIME` environment variable.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# waits for the ramp time before and after injection of chaos 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        # waits for the time interval before and after injection of chaos
        - name: RAMP_TIME
          value: '10' # in seconds
```

### Sequence of chaos execution <a href="#sequence-of-chaos-execution" id="sequence-of-chaos-execution"></a>

The sequence of the chaos execution for multiple targets. Its default value is **parallel**. Tune it by using the `SEQUENCE` environment variable. It supports the following modes:

* `parallel`: The chaos is injected in all the targets at once.
* `serial`: The chaos is injected in all the targets one by one.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# define the order of execution of chaos in case of multiple targets 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        # define the sequence of execution of chaos in case of multiple targets
        # supports: serial, parallel. default: parallel
        - name: SEQUENCE
          value: 'parallel'
```

### Instance ID <a href="#instance-id" id="instance-id"></a>

A user-defined string that holds metadata or information about the current run or instance of chaos. For example, `04-05-2020-9-00`. This string is appended as a suffix in the chaosresult CR name. Tune it by using the `INSTANCE_ID` environment variable.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# provide to append user-defined suffix in the end of chaosresult name 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        # user-defined string appended as suffix in the chaosresult name
        - name: INSTANCE_ID
          value: '123'
```

### Image used by the helper pod <a href="#image-used-by-the-helper-pod" id="image-used-by-the-helper-pod"></a>

The image used to launch the helper pod, if applicable. Tune it by using the `LIB_IMAGE` environment variable. It is supported by **container-kill**, **network-faults**, **stress-faults**, **dns-faults**, **disk-fill**, **kubelet-service-kill**, **docker-service-kill**, and **node-restart** faults.

The following YAML snippet illustrates the use of this environment variable:

```yaml
# it contains the lib image used for the helper pod 
# it support [container-kill, network-faults, stress-faults, dns-faults, disk-fill, 
# kubelet-service-kill, docker-service-kill, node-restart] faults 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: container-kill
    spec:
      components:
        env:
        # name of the lib image
        - name: LIB_IMAGE
          value: 'harness/chaos-go-runner:main-latest'
```

### Default health check <a href="#default-health-check" id="default-health-check"></a>

Determines if you wish to run the default health check which is present inside the fault. Its default value is 'true'. Tune it by using the `DEFAULT_HEALTH_CHECK` environment variable.

The following YAML snippet illustrates the use of this environment variable:

```yaml
## application status check as tunable 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        - name: DEFAULT_HEALTH_CHECK
          value: 'false'
```

### Status check timeout <a href="#status-check-timeout" id="status-check-timeout"></a>

Status check timeout is a configuration parameter that defines the maximum duration the chaos experiment will wait for a resource (like an application under test or a node) to reach the desired state (e.g., Running, Ready) before proceeding.

* If the resource doesn't attain the expected state within this timeframe, the experiment may be marked as failed.

The following YAML snippet illustrates its usage:

```yaml
# contains status check timeout for the experiment pod 
# it will set this timeout as upper bound while checking application status, node status in experiments 
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: engine-nginx
spec:
  engineState: "active"
  annotationCheck: "false"
  appinfo:
    appns: "default"
    applabel: "app=nginx"
    appkind: "deployment"
  chaosServiceAccount: pod-delete-sa
  experiments:
  - name: pod-delete
    spec:
      components:
        # status check timeout for the experiment pod
        statusCheckTimeouts:
          delay: 2
          timeout: 300
```

{% @harness-feedback/feedback %}
