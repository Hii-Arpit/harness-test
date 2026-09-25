---
description: >-
  Supported deployment targets, manifest stores, and artifact sources for
  Harness Deployments.
---


# What's Supported

This page lists the deployment targets, manifest stores, and artifact sources supported in Harness Deployments.

{% tabs %}
{% tab title="Deployments" %}
### Kubernetes <a href="#kubernetes" id="kubernetes"></a>

**Supported deployment strategies**

| Strategy     | Description                                                                             |
| ------------ | --------------------------------------------------------------------------------------- |
| Rolling      | Incrementally replaces pods batch by batch with automatic rollback                      |
| Canary       | Deploys a subset of pods alongside stable pods, then promotes with a rolling deploy     |
| Blue-green   | Maintains two pod sets and swaps service selectors at cutover                           |
| Blank canvas | No managed steps — build your own sequence for Jobs, CronJobs, and supporting resources |

**Supported manifest types**

K8s Manifest, Values YAML, Helm Chart, Kustomize, Kustomize Patches, OpenShift Template

**Supported infrastructure providers**

Kubernetes (direct), Google Kubernetes Engine (GKE), Microsoft Azure (AKS), Amazon Elastic Kubernetes Service (EKS), Rancher

**Other**

| Capability                                | Supported |
| ----------------------------------------- | --------- |
| OpenShift (DeploymentConfig, `oc` client) | ✅         |
| Server-side apply                         | ✅         |
| Manifest pruning                          | ✅         |
| Automatic rollback on failure             | ✅         |
| Kubernetes Steady State Check             | ✅         |
| Harness release history tracking          | ✅         |
| Classic Delegate and [Delegate 3.x](https://developer.harness.io/harness-platform/3.0/harness-platform-resources/delegates)             | ✅         |

***

### Helm <a href="#helm" id="helm"></a>

**Supported deployment strategies**

| Strategy     | Description                                                                               |
| ------------ | ----------------------------------------------------------------------------------------- |
| Basic deploy | Runs `helm upgrade --install` in a single phase                                           |
| Canary       | Deploys a canary Helm release at a specified instance count, then deletes after promotion |
| Blue-green   | Deploys to a stage Helm release, swaps traffic, then cleans up the old release            |

**Supported Helm versions**

| Version | Support |
| ------- | ------- |
| Helm V3 | ✅       |

**Supported chart store types**

Harness Code, GitHub, Git, GitLab, Bitbucket, Azure Repos, Amazon S3, Google Cloud Storage, HTTP Helm

**Other**

| Capability                    | Supported                               |
| ----------------------------- | --------------------------------------- |
| `helm test` after deploy      | ✅                                       |
| Server-side chart rendering   | ✅                                       |
| Automatic rollback on failure | ✅ (via Helm Rollback step)              |
| Ignore failed release history | ✅                                       |
| Manifest pruning              | ❌ (Helm manages release state natively) |
| Classic Delegate and [Delegate 3.x](https://developer.harness.io/harness-platform/3.0/harness-platform-resources/delegates) | ✅                                       |

***

### Google Cloud Run <a href="#google-cloud-run" id="google-cloud-run"></a>

**Supported deployment strategies**

| Strategy | Description |
| -------- | ----------- |
| Basic | Deploys a new Cloud Run revision and routes 100% of traffic to it immediately with automatic rollback on failure |
| Canary | Deploys a new revision and progressively shifts traffic via explicit Traffic Shift steps, with rollback to the pre-shift state |
| Blank canvas | No managed step sequence — compose Deploy, Traffic Shift, Rollback, and Job steps in any order |

**Supported artifact types**

| Artifact type | Supported |
|---|---|
| Google Artifact Registry (container image) | ✅ |
| Docker Hub (container image) | ✅ |

**Supported manifest store types**

Service manifests (Cloud Run YAML / Knative serving spec) can be stored in: Harness Code, GitHub, Git, GitLab, Bitbucket, Azure Repos

**Other**

| Capability | Supported |
|---|---|
| Automatic rollback on failure | ✅ |
| Traffic routing to named revisions | ✅ |
| Cloud Run Jobs (batch / scheduled workloads) | ✅ |
| Instance sync via Google Cloud Monitoring | ✅ |
| Service Account key authentication | ✅ |
| OIDC / Workload Identity authentication | ✅ |
| Inherit from Delegate authentication | ✅ |
| Containerized step execution (`runtime.kubernetes`) | ✅ |
| Harness Cloud runtime | ✅ |

***

### AWS Lambda <a href="#aws-lambda" id="aws-lambda"></a>

**Supported deployment strategies**

| Strategy | Description |
| -------- | ----------- |
| Rolling  | Publishes a new function version and routes 100% of traffic to it immediately with automatic rollback on failure |
| Canary   | Publishes a new function version and progressively shifts traffic via a weighted alias (10% → 100%), with rollback |

**Supported artifact types**

| Artifact type | Rolling | Canary |
|---|---|---|
| Amazon S3 (ZIP) | ✅ | ✅ |
| Amazon ECR (container image) | ✅ | ✅ |
| Artifactory (generic ZIP) | ✅ | ✅ |
| Nexus 3 (raw ZIP) | ✅ | ✅ (runtime-input matrix) |

Nexus 3 is supported for canary only as a ZIP artifact. Container-image canary deployments require ECR.

**Supported manifest store types**

Function definition and alias definition manifests can be stored in: Harness Code, GitHub, Git, GitLab, Bitbucket, Azure Repos, AWS S3

**Other**

| Capability | Supported |
|---|---|
| Automatic rollback on failure (rolling) | ✅ |
| Automatic rollback on failure (canary) | ✅ |
| Function alias management | ✅ (optional for rolling; required for ZIP canary; auto-managed for ECR canary) |
| Secrets in function manifest | ✅ |
| Pipeline-level input expressions | ✅ |
| Multi-service × multi-infra stage matrix (2 × 2 = 4 stages) | ✅ |
| Service runtime inputs (V1 `inputs_yaml` overlay) | ✅ |
| Containerized step execution (`runtime.kubernetes`) | ✅ (required for all Lambda steps) |
{% endtab %}

{% tab title="Manifest Sources" %}
Manifests and config files can be stored in the following sources. The table shows which sources are supported per manifest type.

| Manifest type      | Harness Code | GitHub | Git | GitLab | Bitbucket | Azure Repos | AWS S3 | Google Cloud Storage | HTTP Helm | Custom Remote |
| ------------------ | :----------: | :----: | :-: | :----: | :-------: | :---------: | :----: | :------------------: | :-------: | :-----------: |
| K8s Manifest       |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |        |                      |           |       ✅       |
| Values YAML        |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |    ✅   |                      |           |       ✅       |
| Kustomize          |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |        |                      |           |               |
| Kustomize Patches  |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |        |                      |           |       ✅       |
| OpenShift Template |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |        |                      |           |       ✅       |
| Helm Chart         |       ✅      |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |    ✅   |           ✅          |     ✅     |               |
| Cloud Run Service Manifest |  ✅  |    ✅   |  ✅  |    ✅   |     ✅     |      ✅      |        |                      |           |               |
| Lambda Function Definition / Alias Definition | ✅ |  ✅  |  ✅  |    ✅   |     ✅     |      ✅      |    ✅   |                      |           |               |
{% endtab %}

{% tab title="Artifacts" %}
The table shows which artifact sources are supported per deployment type.

| Deployment type | Harness Artifact Registry | Docker Hub | Amazon ECR | GCR\* | ACR | Artifactory | Nexus 3 | Google Artifact Registry | GitHub Package Registry | Custom |
| --------------- | :-----------------------: | :--------: | :--------: | :---: | :-: | :---------: | :-----: | :----------------------: | :---------------------: | :----: |
| Kubernetes      |             ✅             |      ✅     |      ✅     |   ✅   |  ✅  |      ✅      |    ✅    |             ✅            |            ✅            |    ✅   |
| Helm            |             ✅             |      ✅     |      ✅     |   ✅   |  ✅  |      ✅      |    ✅    |                          |            ✅            |    ✅   |
| Google Cloud Run |                           |      ✅     |            |       |     |             |         |             ✅            |                         |        |
| AWS Lambda      |                           |            |      ✅     |       |     |      ✅      |    ✅    |                          |                         |        |

**GCR** is deprecated. Migrate to Google Artifact Registry.

AWS Lambda also supports **Amazon S3** as a ZIP artifact source (not shown above as S3 is a file store, not a container registry). The full Lambda artifact matrix: S3 ✅, ECR ✅, Artifactory ✅, Nexus 3 ✅.
{% endtab %}
{% endtabs %}

{% @harness-feedback/feedback %}
