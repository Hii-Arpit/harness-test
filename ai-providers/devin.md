# Devin

Connecting Devin brings your Devin spend into Harness Cloud & AI Cost Management alongside your cloud costs. The connector uses a Devin Admin key with read-only access to pull usage and cost data from your Devin account, so you can analyze AI spend in Cost Explorer, attribute it with Views and Cost Categories, and govern it with budgets and anomaly detection, the same workflow you already use for cloud.

### Before You Begin <a href="#before-you-begin" id="before-you-begin"></a>

**Devin Admin key:** An Admin key with read-only access from your Devin account. The key enables Harness to ingest billing and usage data. Go to the [Devin API authentication documentation](https://docs.devin.ai/api-reference/authentication) to generate one, or perform the following steps.

### Create the Devin Admin Key <a href="#create-the-devin-admin-key" id="create-the-devin-admin-key"></a>

Devin recommends a service user key for automated access. To create one:

1. Sign in to Devin and go to **Settings** → **Devin API**. Enterprise deployments use a dedicated host, for example, `your-org.devinenterprise.com`.
2. On the **Devin API** page, open the **Service users** tab, then click **Provision** and select **Enterprise service user** or **Organization service user**, depending on your account.
3. In the dialog box, enter a **Display name**, for example, `Harness CCM Integration`.
4. Assign the **Admin** role, and set **Expiration** to the longest available option (or **Never**) so ingestion does not stop when the key expires.
5. Click **Provision service user**.
6. Copy the generated API key and store it securely. The key starts with `cog_` and is shown only once at creation.

{% hint style="info" %}
Legacy personal API keys are still available under the **Legacy API** tab on the **Devin API** page, but Devin has deprecated them in favor of service user keys.
{% endhint %}

### Set Up the Devin Connector <a href="#set-up-the-devin-connector" id="set-up-the-devin-connector"></a>

To connect Devin, go to **Cloud & AI Cost Management** → **Account Settings** → **AI Providers**, click **AI Provider**, and then select **Devin**.

#### Step 1: Name the Connector <a href="#step-1-name-the-connector" id="step-1-name-the-connector"></a>

1. On the **Overview** panel, enter a **Name** for the connector.
2. Optionally, add a **Description** and **Tags** to organize and filter connectors.
3. Click **Continue**.

#### Step 2: Add the API Key <a href="#step-2-add-the-api-key" id="step-2-add-the-api-key"></a>

On the **Connector Details** panel, provide the Devin Admin key you created in Create the Devin Admin Key as a Harness secret. Do the following:

1. In the **API Key** field, click **Create or Select a Secret**.
   * To use an existing secret that contains your Devin Admin key, select it from the list, then click **Apply Selected**.
   *   To add a new secret, click **New Secret Text**, enter a **Secret Name**, and provide the Devin Admin key you created as the **Secret Value**.

       Go to [Add and reference text secrets](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/secrets/add-use-text-secrets) to review all secret creation options.
2. Click **Continue**.

#### Step 3: Verify the Connection <a href="#step-3-verify-the-connection" id="step-3-verify-the-connection"></a>

On the **Connection Test** panel, Harness validates the API key against your Devin account. Once the verification is successful, click **Finish** to create the connector.
