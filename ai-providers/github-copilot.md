# Github Copilot

Connecting GitHub Copilot brings your GitHub Copilot spend into Harness Cloud & AI Cost Management alongside your cloud costs. The connector uses an Admin key with read-only access to pull usage and cost data from your GitHub Copilot account. You can analyze AI spend in Cost Explorer, attribute it with Views and Cost Categories, and govern it with budgets and anomaly detection, the same way you manage cloud costs.

### Before You Begin <a href="#before-you-begin" id="before-you-begin"></a>

**GitHub Copilot Admin key:** An Admin key with read-only access from your GitHub organization. The key enables Harness to ingest billing and usage data. Go to the [GitHub Copilot metrics API documentation](https://docs.github.com/en/rest/copilot/copilot-metrics) to review the required scopes, or perform the following steps.

### Create the GitHub Copilot Admin Key <a href="#create-the-github-copilot-admin-key" id="create-the-github-copilot-admin-key"></a>

Harness reads the Copilot usage and billing data through a GitHub personal access token (classic).

1. In GitHub, go to **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**, or open [github.com/settings/tokens](https://github.com/settings/tokens) directly.
2. Click **Generate new token** → **Generate new token (classic)**, then provide a descriptive name, for example, `Harness CCM Integration`.
3. Set the **Expiration** to **No expiration** (or the longest available option) so ingestion does not stop when the token expires.
4. Select the scope that matches your account type:
   * **Organization:** `read:org`
   * **Enterprise:** `manage_billing:copilot` or `read:enterprise`
5. Click **Generate token**, then copy it and store it securely. GitHub does not display the token again after creation.

{% hint style="info" %}
You must be an organization or enterprise **owner** or **billing manager** (or hold the fine-grained **View Copilot Metrics** permission), and the **Copilot usage metrics** policy must be enabled for the account before the metrics endpoints return data.
{% endhint %}

### Set Up the GitHub Copilot Connector <a href="#set-up-the-github-copilot-connector" id="set-up-the-github-copilot-connector"></a>

To connect GitHub Copilot, go to **Cloud & AI Cost Management** > **Account Settings** > **AI Providers**, click **AI Provider**, and then select **GitHub Copilot**.

#### Step 1: Name the Connector <a href="#step-1-name-the-connector" id="step-1-name-the-connector"></a>

1. On the **Overview** panel, enter a **Name** for the connector.
2. Optionally, add a **Description** and **Tags** to organize and filter connectors.
3. Click **Continue**.

#### Step 2: Add the API Key <a href="#step-2-add-the-api-key" id="step-2-add-the-api-key"></a>

On the **Connector Details** panel, provide the GitHub Copilot Admin key you created in Create the GitHub Copilot Admin Key as a Harness secret. Do the following:

1. In the **API Key** field, click **Create or Select a Secret**.
   * To use an existing secret that contains your GitHub Copilot Admin key, select it from the list, then click **Apply Selected**.
   *   To add a new secret, click **New Secret Text**, enter a **Secret Name**, and provide the GitHub Copilot Admin key you created as the **Secret Value**.

       Go to [Add and reference text secrets](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/secrets/add-use-text-secrets) to review all secret creation options.
2. Click **Continue**.

#### Step 3: Verify the Connection <a href="#step-3-verify-the-connection" id="step-3-verify-the-connection"></a>

On the **Connection Test** panel, Harness validates the API key against your GitHub Copilot account. Once the verification is successful, click **Finish** to create the connector.
