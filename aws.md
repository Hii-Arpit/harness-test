---
description: Connect AWS to bring your cloud spend into Cloud & AI Cost Management.
tags:
  - cloud-cost-management
title: AWS
---


# AWS

{% hint style="info" %}
**IMPORTANT**

We recommend AWS **CUR 2.0** (Data Exports), which covers all CACM features. **CUR 1.0 (Legacy CUR)** is also fully supported.
{% endhint %}

***

### Before You Start <a href="#before-you-start" id="before-you-start"></a>

To ensure a smooth and error-free setup experience, complete the following steps in your **AWS console** before launching the Harness wizard. This will allow you to progress through the setup without delays or missing prerequisites.

| Required Info                        | Where to Find It                             | Why It’s Needed                                               |
| ------------------------------------ | -------------------------------------------- | ------------------------------------------------------------- |
| **AWS Account ID** (12-digit number) | AWS Console → Account Settings               | Used to associate your cloud costs with your Harness project. |
| **Cost and Usage Report (CUR)**      | AWS Console → Billing → Cost & Usage Reports | Harness uses this to ingest detailed billing data.            |
| **S3 Bucket Name**                   | AWS Console → S3                             | Stores the CUR files for Harness to access.                   |

#### Set Up the Cost and Usage Report <a href="#set-up-the-cost-and-usage-report" id="set-up-the-cost-and-usage-report"></a>

Create the report in your AWS console, then use its name and S3 bucket when you configure the Harness connector. We recommend **CUR 2.0**, but **Legacy CUR** is also fully supported.

{% hint style="info" %}
CUR 2.0 support is enabled via the `CCM_AWS_NEW_CUR` feature flag. Contact [Harness Support](mailto:support@harness.io) to enable it.
{% endhint %}

{% tabs %}
{% tab title="CUR 2.0 (recommended)" %}
1. In the AWS console, navigate to **Billing and Cost Management** → **Data Exports** → **Create export**.
2. Under **Report details**, select all four options:
   * Include resource IDs
   * Split cost allocation data
   * Include caller identity (IAM principal) allocation data
   * Include capacity reservation columns and granularity
3.  Under **Delivery options**, configure the following:

    | Setting              | Required Value                                                                                  |
    | -------------------- | ----------------------------------------------------------------------------------------------- |
    | **Compression type** | Parquet                                                                                         |
    | **S3 bucket**        | Select or create a bucket. Copy the bucket name, as you will need it for the Harness connector. |
    | **S3 path prefix**   | Enter any prefix.                                                                               |
4. Do not uncheck any columns.
5.  Review and create the export. Copy the export name, as you will need it for the Harness connector.

    <figure><img src="../../.gitbook/assets/aws-cur-2-0.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="Legacy CUR" %}
1. In the AWS console, go to **Billing → Cost & Usage Reports** and click **Create report**.
2. Select **Legacy CUR export** and give the report a descriptive name. Copy this name, as you will need it for the Harness connector.
3.  Under **Report details**, configure the following:

    | Setting                  | Required Value            | Notes                                          |
    | ------------------------ | ------------------------- | ---------------------------------------------- |
    | **Include Resource IDs** | ✅ Enabled                 | Must be checked in "Additional report details" |
    | **Time Granularity**     | Hourly                    | Required for accurate cost tracking            |
    | **Report versioning**    | Create new report version | -                                              |
4.  Under **Delivery options**, configure the following:

    | Setting                   | Required Value            | Notes                                          |
    | ------------------------- | ------------------------- | ---------------------------------------------- |
    | **S3 bucket**             | Select or create a bucket | Copy the bucket name for the Harness connector |
    | **S3 path prefix**        | Enter any prefix          | Note it if you set one                         |
    | **Compression**           | GZIP                      | Required format                                |
    | **File Format**           | CSV                       | Parquet is not supported                       |
    | **Data Refresh Settings** | Automatic                 | Enable "Automatically refresh"                 |
5. Review and create the report.
{% endtab %}
{% endtabs %}

**Related Documentation**

* [AWS CUR User Guide](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html)
* [Legacy CUR vs CUR 2.0](https://docs.aws.amazon.com/cur/latest/userguide/table-dictionary-cur2.html)

***

{% hint style="warning" %}
**TIME FOR DATA DELIVERY**

It may take up to **24 hours** for AWS to begin delivering cost and usage data. You can still proceed through the wizard, but the connection test may fail if data isn’t yet available.

In the meantime, explore the optional requirements and feature integrations available in Harness CACM, these will be available to select in your **Choose Requirements** step of the connection wizard:

* [Resource Inventory Management](../../cost-reporting/bi-dashboards/overview/).
* [Optimization by AutoStopping](../../cost-optimization/autostopping-rules/1-auto-stopping-rules.md).
* [Cloud Governance](../../cost-governance/asset-governance/1-asset-governance.md).
* [Commitment Orchestration](../../cost-optimization/commitment-orchestrator/).
{% endhint %}

***

### Interactive Guide <a href="#interactive-guide" id="interactive-guide"></a>

Connect your AWS account to Harness using the connector wizard. Watch the walkthrough below, or follow the [Step-by-Step](aws.md#step-by-step) instructions for the full detail on each step.

{% embed url="https://app.tango.us/app/embed/3bcd4491-b41a-434f-8598-3bf6ca4674b5?skipCover=false&defaultListView=false&skipBranding=false&makeViewOnly=true&hideAuthorAndDetails=true" %}
Add AWS Cloud Cost Connector in Harness
{% endembed %}

### Step-by-Step <a href="#step-by-step" id="step-by-step"></a>

#### Step 1: Add AWS Account Details <a href="#step-1-add-aws-account-details" id="step-1-add-aws-account-details"></a>

1. In the wizard, enter a name for your connector (e.g., `ccm-aws-prod`).
2. Enter your **12-digit AWS Account ID**.
3. (Optional) Add a description and tags to help identify this connector later.
4. If you're using a GovCloud account, select **Yes**; otherwise, leave the default.
5. Click **Continue**.

#### Step 2: Select or Create a Cost and Usage Report <a href="#step-2-select-or-create-a-cost-and-usage-report" id="step-2-select-or-create-a-cost-and-usage-report"></a>

In the connector wizard, select a report type. We recommend **CUR 2.0**, but **CUR 1.0 (legacy)** is also fully supported.

{% tabs %}
{% tab title="CUR 2.0 (recommended)" %}
1. In the connector wizard, select the **CUR 2.0 (recommended)** tab.
2. Click **Launch AWS console** and follow the [CUR 2.0 setup steps](README.md#set-up-the-cost-and-usage-report) to create a Data Export if you have not done so already.
3. Enter the **Data Export Name** and **S3 Bucket Name** in the fields provided.
4. Click **Continue**.

<figure><img src="../../.gitbook/assets/curtwo.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
{% endtab %}

{% tab title="CUR 1.0 (legacy)" %}
1. In the connector wizard, select the **CUR 1.0 (legacy)** tab.
2. Click **Launch AWS console** and follow the [Legacy CUR setup](README.md#legacy-cur-setup) steps to create a report if you have not done so already.
3. Enter the **Cost and Usage Report Name** and **S3 Bucket Name** in the fields provided.
4. Click **Continue**.
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Review [Feature Permissions](../../resources/feature-permissions.md) for CACM to understand the minimum IAM roles or policies needed for every CACM feature.
{% endhint %}

#### Step 3: Choose Requirements <a href="#step-3-choose-requirements" id="step-3-choose-requirements"></a>

1. **Cost Visibility** is selected by default and is required, leave it checked.
2. (Optional) You can enable any of the following features (they can also be added later):
   * Resource Inventory Management
   * Optimization by AutoStopping
   * Cloud Governance
   * Commitment Orchestration
3. Click **Continue**.

{% hint style="info" %}
Not sure which options to choose? [Learn more about each feature](aws.md#before-you-start).
{% endhint %}

#### Step 4: Authentication (Conditional) <a href="#step-4-authentication-conditional" id="step-4-authentication-conditional"></a>

If you have selected **Optimization by AutoStopping**, **Cloud Governance** or **Commitment Orchestration**, in previous step, you can set up Authentication using OIDC. If not selected, this step will not be prompted.

You can enable authentication for your AWS account via

* Cross Account Role: Created with [custom permissions](../../resources/feature-permissions.md)
* [OIDC Authentication](../../resources/oidc-auth.md): Federated access with no stored credentials

{% hint style="info" %}
OIDC Authentication for AWS is behind the `CCM_ENABLE_OIDC_AUTH_AWS` feature flag. Contact [Harness Support](mailto:support@harness.io) to enable it.
{% endhint %}

#### Step 5: Enter Cross Account Role Details <a href="#step-5-enter-cross-account-role-details" id="step-5-enter-cross-account-role-details"></a>

1. Paste the **Cross Account Role ARN** you created via the [CloudFormation template](https://continuous-efficiency.s3.us-east-2.amazonaws.com/setup/v1/ng/HarnessAWSTemplate.yaml). You can find it under **CloudFormation → Stacks → Outputs tab** in AWS.

{% hint style="info" %}
If you are using **CUR 2.0**, ensure the Cross Account IAM role has been updated with the **CUR 2.0** permissions by re-running the [CloudFormation template](https://continuous-efficiency.s3.us-east-2.amazonaws.com/setup/v1/ng/HarnessAWSTemplate.yaml) or manually adding them as described in the [Migrating from CUR 1.0](aws.md#migrating-from-cur-10) section.
{% endhint %}

2. The **External ID** will be pre-filled. Leave it as is.
3. Click **Save and Continue**.

#### Step 6: Verify the Connection <a href="#step-6-verify-the-connection" id="step-6-verify-the-connection"></a>

1. Harness will attempt to validate the connection using your inputs.
2. If this step fails, it's usually because AWS has not yet delivered the first CUR file.
   * Wait up to **24 hours** after setting up the CUR before trying again.
3. Once validated, click **Finish Setup**.

***

🎉 You’ve now connected your AWS account and enabled cost visibility in Harness.

***

### Migrating from CUR 1.0 <a href="#migrating-from-cur-10" id="migrating-from-cur-10"></a>

If you already have an AWS billing connector configured with **CUR 1.0** and want to migrate to **CUR 2.0**:

1. Edit the existing AWS billing connector.
2. In the **Cost and Usage Report** step, select the **CUR 2.0 (recommended)** tab.
3. Update the Cross Account IAM role with the required **CUR 2.0** permissions using one of the following methods:
   * **CloudFormation template (recommended):** Re-run the [CloudFormation template](https://continuous-efficiency.s3.us-east-2.amazonaws.com/setup/v1/ng/HarnessAWSTemplate.yaml), which includes all required permissions for both **CUR 1.0** and **CUR 2.0**.
   *   **Manual:** Add the following permissions directly to the existing Cross Account IAM role:

       ```json
       {
         "Action": [
           "cur:DescribeReportDefinitions",
           "bcm-data-exports:GetExport",
           "bcm-data-exports:ListExports",
           "organizations:Describe*",
           "organizations:List*"
         ]
       }
       ```
4. Ensure the role also has the required S3 bucket permissions and resource-level access for the Data Export location.

{% hint style="info" %}
After you migrate, AWS generates all new billing data in **CUR 2.0** format. Keep the following in mind:

* **Historical data:** Data from before the migration remains in **CUR 1.0** format and is not automatically converted.
* **Backfill:** If you need pre-migration data in **CUR 2.0** format, contact AWS Support. AWS can backfill up to 36 months. Harness recommends requesting at least the current year to maintain uninterrupted reporting in Harness CCM.

Go to [Migration to CUR 2.0 - Cloud Intelligence Dashboards on AWS](https://docs.aws.amazon.com/guidance/latest/cloud-intelligence-dashboards/migration-to-cur.html) to understand the full impact of migrating.
{% endhint %}

***

### Next Steps <a href="#next-steps" id="next-steps"></a>

Once your **AWS billing data** is flowing into Harness, explore these features to enhance your cloud & AI cost management:

* [View and Create Perspectives](https://developer.harness.io/docs/cloud-cost-management/use-ccm-cost-reporting/ccm-perspectives/creating-a-perspective) to visualize cloud usage and trends.
* Create [Budgets and Alerts](../../cost-governance/budgets/create-a-budget.md) to monitor spend thresholds.
* Use [BI Dashboards](../../cost-reporting/bi-dashboards/overview/) to visualize cloud usage and trends.
* Revisit optional integrations you skipped earlier:
  * [Resource Inventory Management](../../cost-reporting/bi-dashboards/overview/).
  * [Optimization by AutoStopping](../../cost-optimization/autostopping-rules/1-auto-stopping-rules.md).
  * [Cloud Governance](../../cost-governance/asset-governance/1-asset-governance.md).
  * [Commitment Orchestration](../../cost-optimization/commitment-orchestrator/).

Take the next step in your cloud & AI cost management journey and turn visibility into action.

{% @harness-feedback/feedback %}
