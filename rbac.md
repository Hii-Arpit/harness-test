---
description: Manage resources, permissions, and roles for Harness Resilience Testing.
tags:
  - resilience-testing
  - rbac
  - governance
---


# RBAC for Resilience Testing

Harness Resilience Testing uses Role-Based Access Control (RBAC) to determine who can view, create, run, and manage resilience resources. Access control is exposed in the Harness UI under a single **Resilience** module category, so the permissions described here apply across the Resilience Testing module rather than to one submodule. Permissions are enforced against the same account, organization, and project hierarchy as the rest of the platform.

RBAC acts as the first layer of safety for resilience testing. It controls configuration-time access to resources such as experiments, ChaosHubs, and infrastructures. Execution-time controls, such as which faults can run against which targets, are handled separately by [ChaosGuard](governance/governance-in-execution/governance-in-execution.md).

{% hint style="warning" %}
**EXECUTE VIA PIPELINE PERMISSION REMOVED**

The `Execute via Pipeline` permission (`chaos_chaosexperiment_executepipeline`) is deprecated and removed. It was deprecated in release `access-control_1.262.0` and marked inactive in `access-control_1.265.0`. Pipeline-based experiment execution now uses the **Execute** permission (`chaos_chaosexperiment_execute`). Update any roles, API calls, or Terraform configurations that still reference `chaos_chaosexperiment_executepipeline`.
{% endhint %}

***

## What you will learn <a href="#what-you-will-learn" id="what-you-will-learn"></a>

* **Resilience resources and permissions:** The resources Harness registers for RBAC and the operations available on each.
* **Permission types:** What each permission grants, including the non-standard verbs such as `Execute`, `Manage`, `Access`, and `Verify/Unverify`.
* **Environment permission:** The permission an environment needs before experiments can run against it. This is a common source of access issues.
* **Example roles:** Two reference roles, a resilience administrator and a resilience tester, that you can recreate for your teams.
* **Role assignment:** How to bind a role to a user.

***

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

This page assumes:

* **Platform RBAC basics:** Familiarity with roles and resource groups. Go to [RBAC in Harness](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control) to understand the permissions model.
* **Role administration access:** To create roles and assign them, you need administrative privileges at the scope where you assign the role (for example, the **Account Admin** role for account-level assignment).

***

## Access scopes <a href="#access-scopes" id="access-scopes"></a>

Resilience resources follow the standard Harness [scope hierarchy](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control#permissions-hierarchy-scopes). Assign permissions at the scope that matches how your teams are organized:

* **Account:** Permissions apply across the entire account.
* **Organization:** Permissions apply to all projects within an organization.
* **Project:** Permissions apply within a single project.

A role combines a set of permissions, and a [resource group](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control/manage-resource-groups) determines the resources those permissions apply to. To view your current permissions, go to **Account/Organization/Project Settings** > **Access Control** > **Roles**, select a role, and open the **Resilience** module.

***

## Resilience resources and permissions <a href="#resilience-resources-and-permissions" id="resilience-resources-and-permissions"></a>

Harness registers the following resources for RBAC under the **Resilience** module. Each resource has its own set of permissions that you assign to roles.

<figure><img src="../.gitbook/assets/resilience-permissions.png" alt="The Resilience module permissions in the Harness role editor, showing each resource and its available operations"><figcaption><p>Click to view full size</p></figcaption></figure>

_The Resilience module permissions in the Harness role editor_

| Resource             | Permissions                                                                               | Description                                                                                                                                                                   |
| -------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Chaos Experiment     | <ul><li>View</li><li>Create / Edit</li><li>Delete</li><li>Execute</li></ul>               | Run, edit, and manage [experiments](../chaos-testing/experiments/). The **Execute** permission covers running an experiment directly and as a step inside a Harness pipeline. |
| Chaos Infrastructure | <ul><li>View</li><li>Create / Edit</li><li>Delete</li></ul>                               | Connect and manage the [infrastructure](../chaos-testing/infrastructure/) where experiments run.                                                                              |
| ChaosHub             | <ul><li>View</li><li>Create / Edit</li><li>Delete</li><li>Access</li><li>Manage</li></ul> | Manage [ChaosHubs](../chaos-testing/chaoshub/) that store reusable resilience artifacts.                                                                                      |
| Chaos Faults         | <ul><li>View</li><li>Create / Edit</li><li>Delete</li></ul>                               | Manage the fault definitions available to experiments.                                                                                                                        |
| Chaos Actions        | <ul><li>View</li><li>Create / Edit</li><li>Delete</li></ul>                               | Manage [actions](../chaos-testing/actions/) that run custom tasks within experiments.                                                                                         |
| Resilience Probe     | <ul><li>View</li><li>Create / Edit</li><li>Delete</li><li>Verify / Unverify</li></ul>     | Manage [probes](../chaos-testing/probes/) that validate steady state during experiments.                                                                                      |
| ChaosGuard Rule      | <ul><li>View</li><li>Create / Edit</li><li>Delete</li></ul>                               | Manage [ChaosGuard](governance/governance-in-execution/governance-in-execution.md) rules that govern fault execution.                                                         |
| DR Tests             | <ul><li>View</li><li>Create / Edit</li><li>Delete</li></ul>                               | Manage [disaster recovery tests](../disaster-recovery-testing/get-started.md).                                                                                                |
| Image Registry       | <ul><li>View</li><li>Create / Edit</li></ul>                                              | Manage the [image registry](image-registry.md) used to pull resilience images.                                                                                                |

***

## Environment permission (required to run experiments) <a href="#environment-permission-required-to-run-experiments" id="environment-permission-required-to-run-experiments"></a>

Experiments run against a Harness **Environment**, which is a shared platform resource rather than a Resilience resource. Granting experiment permissions alone is not enough.

{% hint style="warning" %}
**REQUIRED TO RUN EXPERIMENTS AGAINST A TARGET**

Before a user can run experiments against an environment, the role must grant the **Execute Chaos Experiment** permission (`chaos_environment_executeChaosExperiment`) on that environment, in addition to the **Execute** permission on **Chaos Experiment**. If a user can create an experiment but cannot run it against a target, this missing environment permission is the most common cause.
{% endhint %}

<figure><img src="../.gitbook/assets/environments-permissions.png" alt="Environment permissions showing the Execute Chaos Experiment option enabled"><figcaption><p>Click to view full size</p></figcaption></figure>

_The **Execute Chaos Experiment** permission on a Harness environment_

***

## Permission types <a href="#permission-types" id="permission-types"></a>

Permissions control the specific actions a user can perform on a resource. Most resources use the standard verbs, while a few resources add permissions specific to resilience testing.

**Standard permissions:**

* **View:** Read-only access to the resource.
* **Create / Edit:** Create new resources or modify existing ones.
* **Delete:** Remove the resource.

**Resilience-specific permissions:**

* **Execute:** Run the resource. For **Chaos Experiment**, this single permission covers running an experiment directly and as a step inside a Harness pipeline.
* **Access:** Reference a **ChaosHub** when configuring other resources, such as selecting a hub while building an experiment, without granting edit rights.
* **Manage:** Administrative control of a **ChaosHub**, including its connection and configuration, beyond Create / Edit / Delete. Grant **Manage** only to administrators who own ChaosHub setup.
* **Execute Chaos Experiment:** Run experiments against a Harness **Environment**. This permission is set on the environment, not on the Resilience resource. Go to [Environment permission](rbac.md#environment-permission-required-to-run-experiments) to review the requirement.
* **Verify / Unverify:** Mark a **Resilience Probe** as verified or unverified to indicate whether it is approved for use.

{% hint style="info" %}
**CHAOSHUB MANAGE PERMISSION**

`Manage` is a ChaosHub-specific permission that is broader than `Create / Edit`. Treat it as an administrator-level permission and keep it out of standard tester roles.
{% endhint %}

***

## Example roles <a href="#example-roles" id="example-roles"></a>

Harness provides a built-in resilience administrator role, and you can also create custom roles that match your team responsibilities. The two example roles below are common starting points. Recreate them by selecting the permissions shown, then bind them to users or user groups.

### Resilience administrator <a href="#resilience-administrator" id="resilience-administrator"></a>

A resilience administrator owns the full resilience testing setup. This role grants every permission on Resilience resources, including ChaosHub **Manage** and Resilience Probe **Verify / Unverify**, along with the platform permissions needed to administer projects and shared resources.

| Resource               | Permissions                                    |
| ---------------------- | ---------------------------------------------- |
| Chaos Experiment       | View, Create / Edit, Delete, Execute           |
| Chaos Infrastructure   | View, Create / Edit, Delete                    |
| ChaosHub               | View, Create / Edit, Delete, Access, Manage    |
| Chaos Faults           | View, Create / Edit, Delete                    |
| Chaos Actions          | View, Create / Edit, Delete                    |
| Resilience Probe       | View, Create / Edit, Delete, Verify / Unverify |
| ChaosGuard Rule        | View, Create / Edit, Delete                    |
| DR Tests               | View, Create / Edit, Delete                    |
| Image Registry         | View, Create / Edit                            |
| Environment (platform) | Execute Chaos Experiment                       |

<figure><img src="../.gitbook/assets/role-admin-resilience.png" alt="Resilience permissions for a resilience administrator role with all operations selected"><figcaption><p>Click to view full size</p></figcaption></figure>

_Resilience permissions selected for a resilience administrator role_

A resilience administrator also needs the platform permissions to administer projects, users, and shared resources at the relevant scope. Set those in the **Administrative Functions** and **Shared Resources** sections of the same role.

### Resilience tester <a href="#resilience-tester" id="resilience-tester"></a>

A resilience tester builds and runs experiments but does not administer ChaosHubs or platform settings. This role omits ChaosHub **Manage**, ChaosGuard Rule edits, and Probe **Verify / Unverify**.

| Resource               | Permissions                          |
| ---------------------- | ------------------------------------ |
| Chaos Experiment       | View, Create / Edit, Delete, Execute |
| Chaos Infrastructure   | View, Create / Edit, Delete          |
| ChaosHub               | View, Access                         |
| Chaos Faults           | View, Create / Edit, Delete          |
| Chaos Actions          | View, Create / Edit, Delete          |
| Resilience Probe       | View, Create / Edit, Delete          |
| DR Tests               | View, Create / Edit, Delete          |
| Image Registry         | View, Create / Edit                  |
| Environment (platform) | Execute Chaos Experiment             |

<figure><img src="../.gitbook/assets/role-tester-resilience.png" alt="Resilience permissions for a resilience tester role with a reduced permission set"><figcaption><p>Click to view full size</p></figcaption></figure>

_Resilience permissions selected for a resilience tester role_

A resilience tester typically needs only read-only access in the **Administrative Functions** and **Shared Resources** sections, plus access to any connectors and secrets the experiments use.

***

## Assign a role <a href="#assign-a-role" id="assign-a-role"></a>

<details>

<summary>Assign a resilience role to a user</summary>

1. Select **Account/Organization/Project Settings** (left menu) > **Access Control**.
2. In the **Users** table, select the user profile.
3. Under **Role Bindings**, select **+ Role**.
4. Select the role you created and the resource group it applies to, then select **Apply**.

</details>

To create the role itself first, go to [Manage roles](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control/add-manage-roles) to add a role and [Add resource groups](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control/manage-resource-groups) to scope the resources it applies to.

***

## Load testing access <a href="#load-testing-access" id="load-testing-access"></a>

Load Testing does not register dedicated RBAC resources yet. Access to load testing is governed by the same **Resilience** module category as it gains resources. This section will list Load Testing resources and permissions when they become available.

***

## Related concepts <a href="#related-concepts" id="related-concepts"></a>

* [Governance in execution with ChaosGuard](governance/governance-in-execution/governance-in-execution.md): Restrict which faults run against which targets at execution time.
* [Security in Resilience Testing](../security/index.md): Understand blast radius control and infrastructure permissions.
* [RBAC in Harness](https://developer.harness.io/harness-platform/use-harness-platform/platform-access-control): Review the platform-wide permissions model.

{% @harness-feedback/feedback %}
