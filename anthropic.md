---
description: >-
  Connect Anthropic to bring your Claude spend into Cloud & AI Cost Management
  alongside your cloud costs.
tags:
  - cloud-cost-management
  - ai-cost-management
---


# Anthropic

Connecting Anthropic brings your Claude spend into Harness Cloud & AI Cost Management alongside your cloud costs. The connector uses an API key to pull usage and cost data from your Anthropic account. You can analyze AI spend in Cost Explorer, attribute it with Views and Cost Categories, and govern it with budgets and anomaly detection, the same way you manage cloud costs.

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

**Anthropic API key:** The key type depends on your Anthropic plan - a Console Admin API key for Platform users, or an Analytics API key for Enterprise users. The key enables Harness to ingest billing and usage data. Go to [Create the Anthropic API Key](#create-the-anthropic-api-key) to create the correct key for your plan.

## Create the Anthropic API key <a href="#create-the-anthropic-api-key" id="create-the-anthropic-api-key"></a>

Anthropic bills its Developer Platform and its Enterprise plans separately, and each needs its own type of key. Select the **Anthropic (Platform)** tab if you use the developer API, or the **Anthropic Enterprise** tab for a Team or Enterprise plan.

{% tabs %}
{% tab title="Anthropic (Platform)" %}
1. Sign in to the Claude Console at [platform.claude.com](https://platform.claude.com) using an **Admin** role.
2. Go to **Settings** > **Admin keys**.
3. Click **Create key**, give it a descriptive name (for example, `Harness CCM Integration`), and choose a key expiration.
4. Copy the key and store it securely.

{% hint style="info" %}
Anthropic does not display the key again after creation.
{% endhint %}
{% endtab %}

{% tab title="Anthropic Enterprise" %}
1. Sign in to [claude.ai/admin-settings/api-access](https://claude.ai/admin-settings/api-access) as the **Primary Owner** of your Anthropic Enterprise organization.
2. Toggle on **Enable public API access** if it is not already enabled.
3. Create an Analytics API key and give it a descriptive name, for example, `Harness CCM Integration`. The key is assigned the `read:analytics` scope automatically.
4. Copy the key and store it securely. Anthropic does not display the key again after creation.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
For an **Anthropic Enterprise** plan, use an Analytics API key from `claude.ai/admin-settings/api-access`, **not an Admin API key** from `platform.claude.com`. Enterprise keys require the `read:analytics` scope. Console keys are scope-less (full access) and only work with the Platform product type. Only the **Primary Owner** can generate Enterprise Analytics API keys.
{% endhint %}

## Set up the Anthropic connector <a href="#set-up-the-anthropic-connector" id="set-up-the-anthropic-connector"></a>

To connect Anthropic, go to **Cloud & AI Cost Management** > **Account Settings** > **AI Providers**, click **AI Provider**, and then select **Anthropic**.

### Step 1: Name the connector <a href="#step-1-name-the-connector" id="step-1-name-the-connector"></a>

1. On the **Overview** panel, enter a **Name** for the connector.
2. Optionally, add a **Description** and **Tags** to organize and filter connectors.

<figure><img src="../../.gitbook/assets/anthropic-one.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

3. Click **Continue**.

### Step 2: Select the product type <a href="#step-2-select-the-product-type" id="step-2-select-the-product-type"></a>

On the **Anthropic Product Type** panel, select the Anthropic product your spend comes from:

* **Anthropic (Platform):** The developer API platform, with usage-based billing for API calls to Claude models.
* **Anthropic Enterprise:** The consumer and business chat application, with Team and Enterprise plans on seat-based billing.

Then click **Continue**.

<figure><img src="../../.gitbook/assets/anthropic-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

### Step 3: Add the connector details <a href="#step-3-add-the-connector-details" id="step-3-add-the-connector-details"></a>

On the **Connector Details** panel, confirm the API endpoint and provide the API key you created in [Create the Anthropic API Key](anthropic.md#create-the-anthropic-api-key) as a Harness secret. Do the following:

1.  In the **URL** field, do not change the default `https://api.anthropic.com` unless you have a custom endpoint.

    <figure><img src="../../.gitbook/assets/anthropic-three.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>
2. In the **API Key** field, click **Create or Select a Secret**.
   * To use an existing secret that contains your Anthropic API key, select it from the list, then click **Apply Selected**.
   * To add a new secret, click **New Secret Text**, enter a **Secret Name**, and provide the API key you created as the **Secret Value**.

       <div data-gb-custom-block data-tag="hint" data-style="warning" class="hint hint-warning"><p>The key must match the product type you selected in Step 2, or the connection test fails with a scope mismatch error. If the connection test fails, refer to <a href="anthropic.md#fix-a-scope-mismatch-error">Fix a scope mismatch error</a>.</p></div>

       <figure><img src="../../.gitbook/assets/anthropic-four.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

       Go to [Add and reference text secrets](https://developer.harness.io/harness-ai/use-harness-platform/secrets/add-use-text-secrets) to review all secret creation options.
3. Click **Continue**.

### Step 4: Verify the connection <a href="#step-4-verify-the-connection" id="step-4-verify-the-connection"></a>

On the **Connection Test** panel, Harness validates the API key against your Anthropic account. Once the verification is successful, click **Finish** to create the connector.

<figure><img src="../../.gitbook/assets/anthropic-five.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% hint style="info" %}
If the test fails with an **HTTP 403 "Missing required scope"** error, your key does not match the product type you selected. Go to [Fix a scope mismatch error](anthropic.md#fix-a-scope-mismatch-error) to resolve it.
{% endhint %}

***

## Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

### Fix a scope mismatch error <a href="#fix-a-scope-mismatch-error" id="fix-a-scope-mismatch-error"></a>

The two Anthropic product types use different key types. If the key does not match the product type you selected in Step 2, the connection test fails with an **HTTP 403 "Missing required scope"** error.

| Product type             | Required key type | Created in |
| ------------------------ | ----------------- | ---------- |
| **Anthropic (Platform)** | Console Admin API key (scope-less, full access) | `platform.claude.com` > Settings > Admin keys |
| **Anthropic Enterprise** | Enterprise Analytics API key (`read:analytics` scope) | `claude.ai/admin-settings/api-access` |

If you select **Anthropic Enterprise** but use a Console Admin key, the test reports that `read:analytics` is missing.

If you select **Anthropic (Platform)** but use an Enterprise Analytics key, the test reports a scope mismatch.

To fix it, do one of the following, and then click **Retest**:

* Go back to Step 2 and select the product type that matches your key.
* Create the correct key type for your product type, save it as a Harness secret, and then reselect it in the **API Key** field.

{% @harness-feedback/feedback %}
