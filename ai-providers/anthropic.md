# Anthropic

## Anthropic

Connecting Anthropic brings your Claude spend into Harness Cloud & AI Cost Management alongside your cloud costs. The connector uses an Admin API key to pull usage and cost data from your Anthropic account. You can analyze AI spend in Cost Explorer, attribute it with Views and Cost Categories, and govern it with budgets and anomaly detection, the same way you manage cloud costs.

### Before You Begin <a href="#before-you-begin" id="before-you-begin"></a>

**Anthropic Admin API key:** An Admin key with read-only access from your Anthropic account. The key lets Harness ingest billing and usage data. Go to the [Anthropic Admin API documentation](https://platform.claude.com/docs/en/manage-claude/admin-api) to create one, or follow the steps below.

### Create the Anthropic Admin API Key <a href="#create-the-anthropic-admin-api-key" id="create-the-anthropic-admin-api-key"></a>

Anthropic bills its Developer Platform and its Enterprise plans separately, and each needs its own type of key. Go to the Platform tab if you use the developer API, or the Enterprise tab for a Team or Enterprise plan.

{% tabs %}
{% tab title="Anthropic (Platform)" %}
1. Sign in to the Claude Console at [platform.claude.com](https://platform.claude.com) with the **Admin** role.
2. In the sidebar, go to **API Keys**, then expand the section to find **Admin Keys**.
3. Click **Create Admin Key** and give it a descriptive name, for example, `Harness CCM Integration`.
4. Copy the key and store it securely. Anthropic does not display the key again after creation.
{% endtab %}

{% tab title="Anthropic Enterprise" %}
1. As the **Primary Owner** of your Anthropic Enterprise organization, sign in to [claude.ai/analytics/api-keys](https://claude.ai/analytics/api-keys). Standard Admins and Owners cannot generate Analytics API keys.
2. Click to create a new **Analytics API key**.
3. Give it a descriptive name, for example, `Harness CCM Integration`.
4. Copy the key and store it securely. Anthropic does not display the key again after creation.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Use an Analytics API key from `claude.ai/analytics/api-keys`, not an Admin API key from `platform.claude.com`. The Enterprise product type requires the `read:analytics` scope, and the Platform product type requires the `api:admin` scope.
{% endhint %}

### Set Up the Anthropic Connector <a href="#set-up-the-anthropic-connector" id="set-up-the-anthropic-connector"></a>

To connect Anthropic, go to **Cloud & AI Cost Management** > **Account Settings** > **AI Providers**, click **AI Provider**, and then select **Anthropic**.

#### Step 1: Name the Connector <a href="#step-1-name-the-connector" id="step-1-name-the-connector"></a>

1. On the **Overview** panel, enter a **Name** for the connector.
2. Optionally, add a **Description** and **Tags** to organize and filter connectors.
3. Click **Continue**.

#### Step 2: Select the Product Type <a href="#step-2-select-the-product-type" id="step-2-select-the-product-type"></a>

On the **Anthropic Product Type** panel, select the Anthropic product your spend comes from:

* **Anthropic (Platform):** The developer API platform, with usage-based billing for API calls to Claude models.
* **Anthropic Enterprise:** The consumer and business chat application, with Team and Enterprise plans on seat-based billing.

Then click **Continue**.

#### Step 3: Add the Connector Details <a href="#step-3-add-the-connector-details" id="step-3-add-the-connector-details"></a>

On the **Connector Details** panel, confirm the API endpoint and provide the Anthropic Admin API key you created in Create the Anthropic Admin API Key as a Harness secret. Do the following:

1. In the **URL** field, do not change the default `https://api.anthropic.com` unless you have a custom endpoint.
2. In the **API Key** field, click **Create or Select a Secret**.
   * To use an existing secret that contains your Anthropic Admin API key, select it from the list, then click **Apply Selected**.
   *   To add a new secret, click **New Secret Text**, enter a **Secret Name**, and provide the Anthropic Admin API key you created as the **Secret Value**.

       <div data-gb-custom-block data-tag="hint" data-style="warning" class="hint hint-warning"><p>The key must match the product type you selected in Step 2, or the connection test fails with a scope mismatch error. If the connection test fails, refer to Fix a scope mismatch error.</p></div>

       Go to [Add and reference text secrets](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/secrets/add-use-text-secrets) to review all secret creation options.
3. Click **Continue**.

#### Step 4: Verify the Connection <a href="#step-4-verify-the-connection" id="step-4-verify-the-connection"></a>

On the **Connection Test** panel, Harness validates the API key against your Anthropic account. Once the verification is successful, click **Finish** to create the connector.

{% hint style="info" %}
If the test fails with an **HTTP 403 "Missing required scope"** error, your key does not match the product type you selected. Go to Fix a scope mismatch error to resolve it.
{% endhint %}

***

### Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

**Fix a Scope Mismatch Error**

Each product type needs an Admin key with a specific scope. If the key does not match the product type you selected in Step 2, the connection test fails with an **HTTP 403 "Missing required scope"** error.

| Product type             | Required key scope |
| ------------------------ | ------------------ |
| **Anthropic (Platform)** | `api:admin`        |
| **Anthropic Enterprise** | `read:analytics`   |

If you select **Anthropic Enterprise** but use a Platform key, the test reports that `read:analytics` is missing.

If you select **Anthropic (Platform)** but use an Enterprise key, the test reports that `api:admin` is missing.

To fix it, do one of the following, and then click **Retest**:

* Go back to Step 2 and select the product type that matches your key.
* Create a new Anthropic Admin key with the scope your product type needs, save it as a Harness secret, and then reselect it in the **API Key** field.
