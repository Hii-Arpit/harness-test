---
description: "Get started with Commitment Orchestrator for RDS to automate Reserved Instance and Savings Plan purchases and reduce AWS database costs"
hidden: true
---


# RDS

{% hint style="info" %}
**Behind a Feature Flag**

Commitment Orchestrator for RDS is currently behind a feature flag. Contact [Harness Support](mailto:support@harness.io) to have the `CCM_COMMORCH_RDS` flag enabled for your account.
{% endhint %}

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22RDS%22%2C%22slug%22%3A%22rds%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Frds-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%2C%7B%22label%22%3A%22EC2%22%2C%22slug%22%3A%22ec2%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Fec2-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%2C%7B%22label%22%3A%22Elasticache%22%2C%22slug%22%3A%22elasticache%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Felasticache-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%5D" selectedPlatform="rds"  iconLibrary="https://developer.harness.io/provider-logos" %}

## Before You Begin

To setup Commitment Orchestrator in Harness CACM, you need:

* **Active CACM Connectors**: You must have at least one active cloud connector set up for the cloud providers you want to categorize costs for: Set Up [CACM Connectors](../../../integrations/cloud-providers/aws.md).
* A master account with the right permissions to be added via AWS connector on which you want to enable orchestration. Select the services for which you want to enable orchestration (permissions can be limited to specific service).

<figure><img src="../../../.gitbook/assets/permissions.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Available permissions for RDS:

```
Action:
- 'ce:GetSavingsPlansCoverage'
- 'ce:GetReservationCoverage'
- 'ce:GetSavingsPlansUtilization'
- 'ce:GetDimensionValues'
- 'ce:GetReservationUtilization'
- 'ce:GetSavingsPlansUtilizationDetails'
- 'ce:GetCostAndUsage'
- 'organizations:ListAccounts'
- 'rds:DescribeReservedDBInstances'
- 'rds:DescribeReservedDBInstancesOfferings'
- 'rds:PurchaseReservedDBInstancesOffering'
'Resource: '*'
```

* **For AWS RDS, only RI orchestration is supported**
* **Required Permissions (Read-Only)**: Your Harness user account must belong to a user group with the following role permissions:

<details>

<summary>Required Read-Only Permissions</summary>

To enable visibility, in the master account connector, you need to add the following permissions.

```
"ec2:DescribeReservedInstancesOfferings",
"ce:GetSavingsPlansUtilization",
"ce:GetReservationUtilization",
"ec2:DescribeInstanceTypeOfferings",
"ce:GetDimensionValues",
"ce:GetSavingsPlansUtilizationDetails",
"ec2:DescribeReservedInstances",
"ce:GetReservationCoverage",
"ce:GetSavingsPlansCoverage",
"savingsplans:DescribeSavingsPlans",
"organizations:DescribeOrganization"
"ce:GetCostAndUsage"
```

And to enable actual orchestration, you need to add the following permissions.

```
"ec2:PurchaseReservedInstancesOffering",
"ec2:GetReservedInstancesExchangeQuote",
"ec2:DescribeInstanceTypeOfferings",              
"ec2:AcceptReservedInstancesExchangeQuote",              
"ec2:DescribeReservedInstancesModifications",   
"ec2:ModifyReservedInstances",
"ce:GetCostAndUsage",
"savingsplans:DescribeSavingsPlansOfferings",
"savingsplans:CreateSavingsPlan"
```

For RDS additional permissions are required.

```
"rds:PurchaseReservedDBInstancesOffering",
"rds:DescribeReservedDBInstancesOfferings",
"pricing:GetProducts"
```

</details>

{% hint style="info" %}
**NOTE**

We have rolled out permissions for Elasticache as well. Available permissions for Elasticache:

```
Action:
- 'ce:GetSavingsPlansCoverage'
- 'ce:GetReservationCoverage'
- 'ce:GetSavingsPlansUtilization'
- 'ce:GetDimensionValues'
- 'ce:GetReservationUtilization'
- 'ce:GetSavingsPlansUtilizationDetails'
- 'ce:GetCostAndUsage'
- 'organizations:ListAccounts'
- 'elasticache:DescribeReservedCacheNodes'
- 'elasticache:DescribeReservedCacheNodesOfferings'
- 'elasticache:PurchaseReservedCacheNodesOffering'
Resource: '*'
```
{% endhint %}

***

### Steps to configure:

* Go to Commitment Orchestrator > Setup Orchestrator.

{% tabs %}
{% tab title="Account and Service details" %}
- Specify the **cloud account and service (RDS)** for which you want to enable orchestration. Currently, Commitment Orchestrator supports **AWS Elastic Compute Cloud (EC2)** and **AWS Relational Database Service (RDS)**. Support for other cloud providers is in the works.
-   Specify the **Master Account Connector**. You need to select the master account with the right permissions to be added via connector on which you want to enable orchestration. You can either select an existing connector for your master account or create one.

    Note: even if **"Commitment Orchestrator"** is enabled in Connector Set Up for any other Account except for **Master**, it will not be visible in the connector list in Commitment Orchestrator Setup since Commitment Orchestrator requires **Master Account connector**.

<figure><img src="../../../.gitbook/assets/rds-one.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="Orchestrator Exclusions (Optional)" %}
Commitment Orchestrator provides you with an option to exclude Accounts, Regions and Instances from the orchestration.

* **Account Exclusions**: You can include or exclude specific accounts from the orchestration. The accounts marked as included will be considered for RI Orchestration.
* **Region Exclusions**: You can include or exclude specific regions from the orchestration. All the regions are shown with their coverage and compute spend for making an informed decision
* **Instance Exclusions**: You can include or exclude specific instances from the orchestration.

<figure><img src="../../../.gitbook/assets/rds-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
**NOTE**

The purchases will happen only at master account level and thus will be in turn applicable for child accounts as well. The exclusion list will only be considered for the compute spend calculations and actual RI/SP may be used against the instances if they are part of child accounts.
{% endhint %}
{% endtab %}

{% tab title="Orchestration Preferences" %}
* **Target Coverage:** The maximum percentage of your compute spend that you want covered by Reserved Instances. Any remaining spend will continue to run on On-Demand. The Commitment Orchestrator automatically adjusts coverage levels based on evolving usage patterns.
* **Orchestration Mode:** Select how the orchestrator executes recommended commitment purchases:
  * **Fully Automated**: Commitment purchases are executed automatically without requiring manual approval.
  * **Manual**: All commitment purchases require explicit manual approval before execution, giving you complete control over the process. All the recommendations are visible in the **Actions** tab on the dashboard.

<figure><img src="../../../.gitbook/assets/rds-three.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="Review & Complete" %}
After all the set-up steps, you can review and finalise your inputs.

<figure><img src="../../../.gitbook/assets/rds-four.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}
{% endtabs %}

***

### Overview Screen

The Orchestration Setup page displays a comprehensive list of all Master Accounts with Commitment Orchestrator connector permissions. From this page, users can enable new orchestration setups and view key metrics including Last 30 Days Coverage, Savings, and the current status of each Orchestrator configuration.

<figure><img src="../../../.gitbook/assets/co-overview.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

### Disable Commitment Orchestrator

To disable a Commitment Orchestrator, navigate to **Cloud & AI Cost Management** > **Commitment** > **Orchestration Setup**. Click on the three ellipses for the orchestrator you want to disable > **Manage Orchestration Setup** > **Disable**.

Disabling the Commitment Orchestrator will:

* Stop all automated commitment management for the selected orchestrator
*   Remove existing orchestration configurations

    <figure><img src="../../../.gitbook/assets/disable.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

    <figure><img src="../../../.gitbook/assets/disable-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

***

{% @harness-feedback/feedback %}
