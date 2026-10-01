---
description: "Get started with Commitment Orchestrator for Elasticache to automate Reserved Instance purchases and reduce AWS cache infrastructure costs"
hidden: true
---


# Elasticache

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22RDS%22%2C%22slug%22%3A%22rds%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Frds-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%2C%7B%22label%22%3A%22EC2%22%2C%22slug%22%3A%22ec2%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Fec2-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%2C%7B%22label%22%3A%22Elasticache%22%2C%22slug%22%3A%22elasticache%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fcommitment-orchestrator%2Fget-started%2Felasticache-get-started%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%5D" selectedPlatform="elasticache"  iconLibrary="https://developer.harness.io/provider-logos" %}

## Before you begin

To setup Commitment Orchestrator in Harness CACM, you need:

* **Active CACM Connectors**: You must have at least one active cloud connector set up for the cloud providers you want to categorize costs for: Set Up [CCM Connectors](../../../integrations/cloud-providers/aws.md).
* A master account with the right permissions to be added via AWS connector on which you want to enable orchestration. Select the services for which you want to enable orchestration (permissions can be limited to specific service).

Below is the Elasticache permissions required:

```yaml
HarnessCommitmentElastiCachePolicy:
    Type: 'AWS::IAM::ManagedPolicy'
    Condition: CreateHarnessCommitmentElastiCachePolicy
    Properties:
      Description: Policy granting Harness Access to ElastiCache Reserved Instances
      PolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
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

***

### Steps to configure:

* Go to Commitment Orchestrator > Setup Orchestrator.

{% tabs %}
{% tab title="Account and Service details" %}
- Specify the **cloud account and service (Elasticache)** for which you want to enable orchestration. Currently, Commitment Orchestrator supports **AWS Elastic Compute Cloud (EC2)**, **AWS Relational Database Service (RDS)**, and **AWS Elasticache**. Support for other cloud providers is in the works.
-   Specify the **Master Account Connector**. You need to select the master account with the right permissions to be added via connector on which you want to enable orchestration. You can either select an existing connector for your master account or create one.

    Note: even if **"Commitment Orchestrator"** is enabled in Connector Set Up for any other Account except for **Master**, it will not be visible in the connector list in Commitment Orchestrator Setup since Commitment Orchestrator requires **Master Account connector**.

<figure><img src="../../../.gitbook/assets/ec-step-one.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="Orchestrator Exclusions (Optional)" %}
Commitment Orchestrator provides you with an option to exclude Accounts, Regions and Instances from the orchestration.

* **Account Exclusions**: You can include or exclude specific accounts from the orchestration. The accounts marked as included will be considered for RI Orchestration.
* **Region Exclusions**: You can include or exclude specific regions from the orchestration. All the regions are shown with their coverage and compute spend for making an informed decision

<figure><img src="../../../.gitbook/assets/ec-step-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

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

<figure><img src="../../../.gitbook/assets/ec-step-three.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

* **Notifications**: Configure alerts to stay informed about commitment activities:
  * **Purchase Notifications**: Get notified about successful RI/SP purchases made by Harness.
  * **Pending Approval Notifications**: Get notified when manual approval is required for RI/SP recommendations.
  * **Savings Plans Expiry Notifications**: Receive notifications for existing Savings Plans that will expire within a specified timeframe.
  * **Daily Summary Delivery**: Set your preferred timezone and email recipients for daily summary reports.

<figure><img src="../../../.gitbook/assets/ec-step-four.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="Review & Complete" %}
After all the set-up steps, you can review and finalise your inputs.

<figure><img src="../../../.gitbook/assets/ec-step-five.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}
{% endtabs %}

***

### Overview screen

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
