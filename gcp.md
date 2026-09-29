---
description: Connect GCP to bring your cloud spend into Cloud & AI Cost Management.
tags:
  - cloud-cost-management
title: GCP
---


# GCP

### Before You Start <a href="#before-you-start" id="before-you-start"></a>

To ensure a smooth and error-free setup experience, set up GCP Billing Export before launching the Harness wizard. This will allow you to progress through the setup without delays or missing prerequisites.

**Why is Billing Export required?**

Harness Cloud & AI Cost Management (CACM) analyzes your cloud spending by accessing detailed billing data from your GCP account. The billing export automatically sends your cost and usage data to BigQuery, where Harness can securely read it.

{% hint style="info" %}
⚠️ GCP Billing Export Table Limitations and CMEK Restrictions When setting up a connector for GCP Billing Export, keep the following limitations and guidelines in mind:

* GCP does not support copying data across regions if the source table uses CMEK.
* GCP also does not allow copying data from Materialized Views.
* If your organization enforces CMEK policies, consider creating a new dataset without CMEK enabled specifically for the integration.
* It is recommended to create the dataset in the US region, as it offers the most compatibility with GCP billing export operations.
* Use datasets with default (Google-managed) encryption when configuring the connector.
{% endhint %}

***

#### Step 1: Set Up the Billing Export <a href="#step-1-set-up-the-billing-export" id="step-1-set-up-the-billing-export"></a>

1. Go to **Billing → Billing export**.
2. Enable **Export detailed billing data to BigQuery**.
3. Choose a billing-enabled project and dataset.
4. Enable:
   * 📅 **Daily cost detail**
   * 📊 Export type: `Detailed usage cost data`
5. Note the **Dataset Name** and **Table Name** for the wizard.

[More Help → GCP Billing Export Guide](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-setup)

***

#### **Step 2: Grant Permissions** <a href="#step-2-grant-permissions" id="step-2-grant-permissions"></a>

1. Go to **BigQuery → Your Project → Dataset**.
2. Click **Share Dataset**.
3. Add the following service account as a **Viewer**: `<account-id>@<project-id>.iam.gserviceaccount.com`

***

{% hint style="warning" %}
**TIME FOR DATA DELIVERY**

It may take up to **24 hours** for GCP to begin delivering cost and usage data. You can still proceed through the wizard, but the connection test may fail if data isn’t yet available.

In the meantime, explore the optional requirements and feature integrations available in Harness CACM, these will be available to select in your **Choose Requirements** step of the connection wizard:

* [Resource Inventory Management](../../cost-reporting/bi-dashboards/overview/).
* [Optimization by AutoStopping](../../cost-optimization/autostopping-rules/1-auto-stopping-rules.md).
* [Cloud Governance](../../cost-governance/asset-governance/1-asset-governance.md).
{% endhint %}

***

### Interactive Guide <a href="#interactive-guide" id="interactive-guide"></a>

Connect your GCP account to Harness using the connector wizard. Watch the walkthrough below, or follow the [Step-by-Step Guide](gcp.md#step-by-step-guide) for the full detail on each step.

{% embed url="https://app.tango.us/app/embed/3eb1eed3-85aa-4b1a-b4e6-d249989e7ce5?skipCover=false&defaultListView=false&skipBranding=false&makeViewOnly=true&hideAuthorAndDetails=true" %}
Add GCP Cloud Cost Connector in Harness
{% endembed %}

### Step-by-Step Guide <a href="#step-by-step-guide" id="step-by-step-guide"></a>

#### Step 1: Add GCP Account Details <a href="#step-1-add-gcp-account-details" id="step-1-add-gcp-account-details"></a>

1. In the wizard, enter a name for your connector (e.g., `gcp-demo-prod`).
2. Specify **Project ID**.
3. (Optional) Add a description and tags to help identify this connector later.
4. Click **Continue**.

#### Step 2: Select or Create a Billing Export <a href="#step-2-select-or-create-a-billing-export" id="step-2-select-or-create-a-billing-export"></a>

Cloud Billing export to BigQuery enables you to export detailed Google Cloud billing data (such as usage and cost estimate data) automatically throughout the day to a BigQuery dataset that you specify.

1. If your Billing Export already exists, select it from the list.
2. If not, return to GCP and follow the steps in the [Before You Start](gcp.md#before-you-start) section to create one.
3. Once the Billing Export appears in the list, select it and click **Continue**.

#### Step 3: Choose Requirements <a href="#step-3-choose-requirements" id="step-3-choose-requirements"></a>

1. **Cost Visibility** is selected by default and is required.
2. (Optional) You can enable any of the following features (they can also be added later):
   * Resource Inventory Management
   * Optimization by AutoStopping. If selected, you can select granular permissions for AutoStopping by clicking **Continue**
   * Cloud Governance
3. Click **Continue**.

#### Step 4: Authentication (Conditional) <a href="#step-4-authentication-conditional" id="step-4-authentication-conditional"></a>

If you have selected **Optimization by AutoStopping** or **Cloud Governance**, in previous step, you can set up Authentication. If not selected, this step will not be prompted.

You can enable authentication for your GCP account via

* Service Account with Custom Role: Created with [custom permissions](../../resources/feature-permissions.md)
* [OIDC Authentication](../../resources/oidc-auth.md): Federated access with no stored credentials

{% hint style="info" %}
OIDC Authentication for GCP is behind the `CCM_ENABLE_OIDC_AUTH_GCP` feature flag. Contact [Harness Support](mailto:support@harness.io) to enable it.
{% endhint %}

#### Step 5: Grant Permissions <a href="#step-5-grant-permissions" id="step-5-grant-permissions"></a>

Based on what you selected in **Step 3 - Choose Requirements**, you will be prompted to grant permissions to your service account alongwith the steps to be followed.

{% hint style="info" %}
Review [Feature Permissions](../../resources/feature-permissions.md) for CACM to understand the minimum roles or permissions needed for every CACM feature.
{% endhint %}

#### Step 6: Verify the Connection <a href="#step-6-verify-the-connection" id="step-6-verify-the-connection"></a>

1. Harness will attempt to validate the connection using your inputs.
2. If this step fails, it's usually because GCP has not yet delivered the first billing export.
   * Wait up to **24 hours** after setting up the billing export before trying again.
3. Once validated, click **Finish Setup**.

***

🎉 You’ve now connected your GCP account and enabled cost visibility in Harness.

***

### Next Steps <a href="#next-steps" id="next-steps"></a>

Once your data is flowing, explore the tools available in Harness CACM to help you manage and reduce your cloud spend:

* [View and Create Perspectives](https://developer.harness.io/docs/cloud-cost-management/use-ccm-cost-reporting/ccm-perspectives/creating-a-perspective) to visualize cloud usage and trends.
* Create [Budgets and Alerts](../../cost-governance/budgets/create-a-budget.md) to monitor spend thresholds.
* Use [BI Dashboards](../../cost-reporting/bi-dashboards/overview/) to visualize cloud usage and trends.
* Revisit optional integrations:
  * [Resource Inventory Management](../../cost-reporting/bi-dashboards/overview/).
  * [Optimization by AutoStopping](../../cost-optimization/autostopping-rules/1-auto-stopping-rules.md).
  * [Cloud Governance](../../cost-governance/asset-governance/1-asset-governance.md).

{% @harness-feedback/feedback %}
